# Ingestion Component

## Purpose
This component is responsible for handling the initial intake of GPS data into the system. It simulates a fleet without requiring hardware by allowing the upload of CSV files containing pre-recorded GPS pings.

## User Story
As a DevOps engineer, I want to upload a CSV file containing pre-recorded GPS pings so that we can simulate a fleet without hardware.

## Acceptance Criteria
- Given a CSV that matches the documented schema,
- When I upload it to the S3 “ingest” bucket,
- Then the file is versioned and a “file-received” EventBridge event is emitted.

## Key Functionality
-   **CSV Upload:** Allows users (specifically DevOps engineers or automated processes) to upload CSV files.
-   **S3 Integration:** Uploaded files are stored in an S3 bucket designated for ingestion.
-   **File Versioning:** S3 bucket will be configured to keep versions of uploaded files.
-   **Event Emission:** Upon successful file upload and versioning, an event is published to AWS EventBridge, specifically a "file-received" event. This event will likely trigger downstream processes.

## Schema
(Details of the CSV schema will be documented here once finalized.)
