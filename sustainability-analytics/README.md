# Sustainability Analytics & Eco-Tips

## Purpose
This component focuses on promoting sustainable driving habits. It uses AWS Bedrock to generate personalized, bite-sized eco-tips for drivers based on their driving behavior. These tips are then logged for review.

## User Stories
1.  **Eco-Tip Generation**: As a sustainability analyst, I want Bedrock to generate bite-sized eco-tips based on driving behaviour so that drivers learn how to save fuel.
2.  **Tip Logging**: As a fleet manager, I want the tips logged against each trip so that I can review driver coaching history.

## Acceptance Criteria
-   **AC-1 (Eco-Tip Generation):**
    -   Agent prompt template for Bedrock is stored in AWS Parameter Store.
    -   Given `speedVariance > 15 kph` (as an example input metric), when the tip is generated, then 80% of tips mention “steady acceleration”.
-   **AC-2 (Tip Logging):**
    -   Tips are written to DynamoDB with `tripId` as the partition key.
    -   An API endpoint `/trips/{id}/tips` returns an array of tips for a given trip, ordered by timestamp.

## Key Functionality
-   **Behavior Analysis:** (Details to be defined) Consumes driving behavior data (e.g., speed variance, harsh braking/acceleration events).
-   **Bedrock Integration:**
    -   Retrieves a prompt template from AWS Parameter Store.
    -   Formats the prompt with specific driving behavior data.
    -   Invokes a Bedrock model to generate an eco-tip.
-   **Tip Storage:**
    -   Persists generated tips in a DynamoDB table.
    -   Uses `tripId` as the partition key for efficient querying of tips per trip.
    -   Includes a timestamp for ordering.
-   **API for Tip Retrieval:**
    -   Provides an API endpoint (e.g., via API Gateway and Lambda) to fetch tips for a specific trip.
    -   Ensures tips are returned in chronological order.

## Prompt Engineering Notes
-   The prompt template in Parameter Store will be crucial for generating relevant and effective tips.
-   It will need to be designed to guide the Bedrock model to focus on actionable advice related to the input driving behavior metrics.
-   The example (`speedVariance > 15 kph` leading to "steady acceleration" tips) illustrates the desired specificity.

## Data Models
-   **DynamoDB Tip Record:**
    -   `tripId` (String, Partition Key)
    -   `timestamp` (Number or String ISO8601, Sort Key if needed for complex queries, or just an attribute for ordering)
    -   `tipText` (String)
    -   `triggeringBehavior` (Map, optional, e.g., `{ "metric": "speedVariance", "value": 18 }`)
