# Driver Mobile App

## Purpose
This component provides a mobile-friendly web interface for drivers. It displays their current route, highlights updates in real-time, and shows recent eco-driving tips. The focus is on a distraction-free UI.

## User Stories
1.  **Route Updates**: As a driver, I want my mobile view to highlight the updated route in ≤ 5 s after optimisation so that I can adjust without confusion.
2.  **Responsive UI & Eco-Tips**: As a driver, I want a responsive mobile page with only my vehicle’s route and last three eco-tips so that the UI is distraction-free.

## Acceptance Criteria
-   **AC-1 (Route Updates):** Given a route change, when the driver’s device receives a push event (or polls for an update), the new polyline is rendered in MapLibre within 5 seconds, and the old path is greyed out.
-   **AC-2 (Responsive UI & Eco-Tips):**
    -   Route polyline, next waypoint, and the last three eco-tips fit on a 390 × 844 screen (common mobile viewport).
    -   Lighthouse mobile score ≥ 85.

## Key Features
-   **Map Display:** Uses MapLibre (or a similar mapping library) to render vehicle routes.
-   **Real-time Updates:**
    -   Listens for push events (e.g., via WebSockets or an equivalent AWS service like IoT Core or AppSync) or periodically polls for route changes.
    -   Updates the map display promptly when a new route is received.
    -   Visually differentiates new routes from old ones (e.g., new route in a prominent color, old route greyed out).
-   **Eco-Tips Display:** Shows the last three eco-driving tips relevant to the driver/trip.
-   **Responsive Design:** Ensures the UI is optimized for mobile devices, specifically targeting a 390x844 viewport.
-   **Performance:** Aims for a high Lighthouse score for mobile performance, accessibility, and best practices.

## Technology Stack (Proposed)
-   **Frontend Framework:** (e.g., React, Vue, Angular, or plain HTML/CSS/JS)
-   **Mapping Library:** MapLibre GL JS
-   **Real-time Communication:** (e.g., AWS AppSync Subscriptions, AWS IoT Core for MQTT, or traditional WebSockets)

## UI/UX Notes
-   Minimalist design to reduce driver distraction.
-   Clear visual hierarchy for route information and tips.
