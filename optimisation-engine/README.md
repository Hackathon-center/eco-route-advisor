# Optimisation Engine

## Purpose
This component is responsible for recomputing optimal vehicle routes to minimize fuel burn by avoiding congestion. It leverages SageMaker Geospatial for these calculations.

## User Story
As the optimisation engine, I want to call SageMaker Geospatial every 60 s to recompute shortest paths that avoid congestion so that fuel burn is minimised.

## Acceptance Criteria
-   **AC-1:** Replan requests sent to SageMaker Geospatial include the current coordinates of the vehicle and its destination.
-   **AC-2:** The response received from SageMaker Geospatial returns a polyline representing the new path and an `estimatedLitresSaved` field.

## Key Functionality
-   **Scheduled Operations:** The engine is triggered every 60 seconds to perform route optimisation. This could be managed by a service like AWS Lambda with a CloudWatch Events trigger.
-   **SageMaker Geospatial Integration:**
    -   Constructs requests to SageMaker Geospatial, providing necessary inputs like current vehicle location (latitude, longitude) and destination.
    -   Processes responses from SageMaker Geospatial to extract the new route (polyline) and estimated fuel savings.
-   **Data Handling:**
    -   Retrieves current vehicle locations and destinations (likely from a database or state store).
    -   Stores or communicates the updated route information and savings, possibly by updating a database and/or emitting an event for other services (e.g., the Driver Mobile App).

## Architecture Notes
(Details about the specific SageMaker Geospatial APIs used, data flow, and how this engine interacts with other components will be added here.)
