# Security

## Purpose
This section details the security measures implemented within the EcoRoute Advisor project, with a primary focus on ensuring data is encrypted at rest using AWS Key Management Service (KMS).

## User Story
As a security officer, I want all location data encrypted at rest with AWS-managed KMS keys so that we meet basic compliance.

## Acceptance Criteria
-   **AC-1:** S3 buckets (e.g., the "ingest" bucket) and DynamoDB tables (e.g., tables storing trip data, eco-tips) have server-side encryption enabled, configured to use an AWS-managed KMS key (SSE-KMS with default `aws/s3` and `aws/dynamodb` keys, or a customer-managed key if specified). The example specifically mentions `AES-256` which is the algorithm used by SSE-S3 (AWS-managed keys for S3) and default for DynamoDB with AWS owned keys. If a specific KMS CMK is to be used, it should be specified. For now, we'll assume AWS-managed keys are sufficient.
-   **AC-2:** The KMS key policy (if a customer-managed key is used) restricts cross-account access, allowing access only to necessary IAM roles and users within the same AWS account. (If using AWS-managed keys like `aws/s3` or `aws/dynamodb`, their policies are managed by AWS and generally don't allow cross-account access unless explicitly granted through bucket/table policies or IAM resource policies).

## Encryption Strategy

### Data at Rest
-   **Amazon S3 Buckets:**
    -   The S3 bucket used for CSV ingestion (`ingest` bucket) will have Server-Side Encryption (SSE) enabled.
    -   By default, this can be SSE-S3 (using AES-256 encryption with keys managed by AWS S3) or SSE-KMS (using AWS KMS to manage encryption keys).
    -   The user story mentions "AWS-managed KMS keys." This typically refers to AWS managed Customer Master Keys (CMKs) or the AWS owned keys for services. For S3, `aws/s3` is the AWS managed key. For DynamoDB, it's `aws/dynamodb`.
    -   We will ensure that the `ingest` bucket is configured for server-side encryption using `AES-256` (which is what SSE-S3 provides, or SSE-KMS with an AWS-managed key).
-   **Amazon DynamoDB Tables:**
    -   All DynamoDB tables storing sensitive information (e.g., trip data, vehicle locations, eco-tips) will have server-side encryption enabled.
    -   DynamoDB offers default encryption at rest using AWS owned keys at no additional charge.
    -   Alternatively, an AWS managed KMS key (`aws/dynamodb`) or a customer-managed KMS key can be specified for encryption.
    -   We will ensure encryption is active, defaulting to AWS owned keys or specifying `aws/dynamodb` if explicit KMS key selection is shown.
-   **Amazon Location Service Trackers:**
    -   Data stored in Amazon Location Service Trackers is encrypted at rest using AWS owned encryption keys.

### Key Management
-   **AWS Key Management Service (KMS):**
    -   For services where we explicitly choose SSE-KMS, we will use AWS-managed keys (e.g., `alias/aws/s3`, `alias/aws/dynamodb`) unless a specific Customer Managed Key (CMK) is required.
    -   If a CMK is created and used:
        -   The key policy will be configured to follow the principle of least privilege.
        -   It will restrict access to IAM roles and services within the AWS account that require encryption/decryption permissions.
        -   Cross-account access will be explicitly denied unless there's a justified requirement (which is not indicated in the current user stories).

## Implementation via AWS CDK (Example)
When defining resources like S3 buckets or DynamoDB tables in AWS CDK, encryption properties will be explicitly set.

**S3 Bucket Example (using SSE-S3 - AWS-managed key):**
```typescript
// Example in AWS CDK (TypeScript)
new s3.Bucket(this, 'IngestBucket', {
  encryption: s3.BucketEncryption.S3_MANAGED, // SSE-S3 (AES-256)
  versioned: true, // As per ingestion user story
});
```

**S3 Bucket Example (using SSE-KMS with AWS-managed s3 key):**
```typescript
// Example in AWS CDK (TypeScript)
new s3.Bucket(this, 'IngestBucket', {
  encryption: s3.BucketEncryption.KMS_MANAGED, // SSE-KMS using aws/s3 key by default
  versioned: true,
});
```

**DynamoDB Table Example (default AWS owned key):**
```typescript
// Example in AWS CDK (TypeScript)
new dynamodb.Table(this, 'TripTable', {
  partitionKey: { name: 'tripId', type: dynamodb.AttributeType.STRING },
  // encryption: dynamodb.TableEncryption.AWS_MANAGED (this is default)
});
```

**DynamoDB Table Example (AWS managed KMS key `aws/dynamodb`):**
```typescript
// Example in AWS CDK (TypeScript)
new dynamodb.Table(this, 'TipTable', {
  partitionKey: { name: 'tripId', type: dynamodb.AttributeType.STRING },
  sortKey: { name: 'timestamp', type: dynamodb.AttributeType.STRING },
  encryption: dynamodb.TableEncryption.AWS_MANAGED, // Corresponds to aws/dynamodb KMS key
});
```

Further details on specific KMS key IDs and policies will be documented if Customer Managed Keys (CMKs) are implemented.
