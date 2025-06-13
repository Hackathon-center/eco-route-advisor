# QA Tools

## Purpose
This directory contains tools and resources for Quality Assurance (QA) testing and for setting up demonstration environments quickly.

## User Story
As a QA tester, I want a Postman collection that seeds sample files so that any team-mate can spin up a demo in < 5 min.

## Acceptance Criteria
-   **AC-1:** The Postman collection is checked into GitHub with this README.
-   **AC-2:** Running “Seed Demo Fleet” (a request or set of requests within the Postman collection) results in at least 10 vehicles appearing on the dashboard within 30 seconds.

## Components
-   **Postman Collection:** A Postman collection will be provided here (`<collection_name>.postman_collection.json`). This collection will include requests to:
    -   Seed the system with sample data (e.g., uploading sample CSV files via the ingestion component).
    -   Potentially, verify endpoints or trigger specific workflows for testing purposes.
-   **Sample Data:** Any necessary sample data files (e.g., sample CSVs for fleet simulation) used by the Postman collection might also be stored here or in a dedicated `sample-data` subdirectory.

## Setup and Usage
(Instructions on how to import the Postman collection and run the "Seed Demo Fleet" requests will be added here.)
