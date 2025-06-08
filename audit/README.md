# Audit & Access Logging

## Purpose
This section describes the mechanisms for auditing access to data within the EcoRoute Advisor system, specifically focusing on enabling AWS CloudTrail Lake for log retention and querying capabilities.

## User Story
As a privacy-conscious fleet manager, I want to view audit logs of who accessed trip data so that I can investigate incidents.

## Acceptance Criteria
-   **AC-1:** AWS CloudTrail Lake is enabled with a 90-day retention period for event data.
-   **AC-2:** A query performed in CloudTrail Lake for `GetItem` API calls on the `TripTable` (the DynamoDB table storing trip data) by a specific `principalId` returns results in < 30 seconds.

## Audit Strategy

### AWS CloudTrail Lake
-   **Enablement:** An AWS CloudTrail Lake event data store will be configured.
-   **Data Events:** The event data store will be configured to include data events, specifically for:
    -   Amazon DynamoDB: To log item-level access like `GetItem`, `PutItem`, `DeleteItem` on tables such as `TripTable`.
    -   Amazon S3: To log object-level access for buckets like the `ingest` bucket.
-   **Retention:** The event data store will be set up with a 90-day retention period for the collected audit logs.
-   **Querying:** Fleet managers or authorized personnel will be able to query these logs using SQL-like syntax directly in the CloudTrail Lake console.

### Focus on Trip Data Access
-   The primary concern is auditing access to `TripTable` in DynamoDB, which presumably stores sensitive trip information.
-   Queries will be designed to filter for:
    -   `eventName: GetItem` (and potentially other read/write operations like `BatchGetItem`, `Query`, `Scan`).
    -   `resources.ARN` (matching the ARN of `TripTable`).
    -   `userIdentity.principalId` (to identify who performed the action).

### Example CloudTrail Lake Query
This is a conceptual example of what a query might look like to satisfy AC-2. The exact field names and structure might vary based on the actual CloudTrail event schema.

```sql
SELECT
    eventID,
    eventName,
    eventTime,
    userIdentity.principalId,
    requestParameters.tableName,
    requestParameters.key
FROM
    <your_event_data_store_id>
WHERE
    eventSource = 'dynamodb.amazonaws.com'
    AND eventName = 'GetItem'
    AND requestParameters.tableName = 'TripTable' -- Or the actual name of the table
    AND userIdentity.principalId = 'AROAXXXXXXXXXXXXXXXXXXXXX' -- Example Principal ID
    AND eventTime > 'YYYY-MM-DDTHH:MM:SSZ' -- To scope the query if needed
ORDER BY
    eventTime DESC;
```

### Implementation Notes
-   **CDK Configuration:** If possible, CloudTrail Lake and its event data store configurations (including data event selectors) will be defined using AWS CDK.
-   **IAM Permissions:** Ensure that appropriate IAM permissions are in place for users or roles who need to query CloudTrail Lake.
-   **Performance:** While AC-2 specifies < 30 seconds query time, actual performance can depend on the volume of data and query complexity. Efficient query design is important.
-   **Cost:** Be mindful of CloudTrail Lake costs, which are based on ingestion and retention. Enabling data events can significantly increase the volume of logs.

This setup will provide the necessary visibility for fleet managers to investigate incidents by reviewing who accessed specific trip data.
