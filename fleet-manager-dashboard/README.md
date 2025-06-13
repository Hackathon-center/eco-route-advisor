# Fleet Manager Dashboard

## Purpose
This component provides a web-based dashboard for dispatchers and fleet managers. It displays real-time vehicle positions, planned routes (original vs. eco-friendly), eco-tips, and key performance indicators like fuel and CO₂ savings. It's also intended to show a comparison of Edge vs. Region replan times.

## User Stories
1.  **Tip Logging Review**: As a fleet manager, I want the tips logged against each trip so that I can review driver coaching history. (This is primarily handled by `sustainability-analytics` for storage and API, but the dashboard will consume and display this data).
2.  **Real-time ROI Dashboard**: As a dispatcher, I want a web dashboard that shows current vehicle positions, planned routes and litres saved so that I see ROI in real time.

## Acceptance Criteria
-   **AC-1 (Dashboard Updates):** Dashboard updates vehicle positions, routes, and metrics without requiring a manual refresh (leveraging AWS AppSync GraphQL subscriptions).
-   **AC-2 (Metrics):** Metric card shows “Total CO₂ avoided (kg)” and matches the sum emitted to CloudWatch.
-   **(Implied from Project Overview)** Displays:
    -   Coloured truck icons moving on a map.
    -   Green "eco" route vs. purple "original" route.
    -   Running counters for litres & kg CO₂ saved.
    -   A small chart comparing Edge vs. Region replan times.
    -   List of vehicles.
    -   Eco-tip pop-ups (potentially near vehicles or in a dedicated section).

## Key Features
-   **Map Display:**
    -   Uses MapLibre GL JS to render vehicle positions and routes.
    -   Shows distinct visual representations for the original route and the optimized "eco" route.
    -   Displays animated truck icons at their current GPS locations.
-   **Real-time Data:**
    -   Subscribes to AWS AppSync (GraphQL) for real-time updates on vehicle positions, new routes, eco-tips, and calculated savings.
    -   Ensures the dashboard reflects the latest information automatically.
-   **Metrics Display:**
    -   Clearly presents "Litres Saved" and "Total CO₂ avoided (kg)".
    -   Includes a comparison chart for "Edge vs. Region" route replan latency.
-   **Vehicle Information:** Lists vehicles in the fleet.
-   **Eco-Tips Visualization:** Displays eco-tips, possibly as pop-up notifications or in a dedicated panel.
-   **Driver Coaching History:** Allows fleet managers to review logged eco-tips for specific trips.

## Technology Stack
-   **Frontend Framework:** Next.js
-   **Mapping Library:** MapLibre GL JS
-   **Real-time API:** AWS AppSync (GraphQL Subscriptions)
-   **Data Source:** DynamoDB (via AppSync)

## UI/UX Notes
-   The dashboard should provide a clear, at-a-glance overview of fleet status and performance.
-   Designed for desktop use by dispatchers and fleet managers.
-   Will serve as a central point for judges to see the sustainability payoff and the telco edge advantage.
