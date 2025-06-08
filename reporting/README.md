# Reporting & Monitoring

## Purpose
This component is dedicated to monitoring and reporting on system performance, with a specific focus on comparing the latency of route replanning requests processed by the AWS Wavelength Zone versus the standard AWS Region.

## User Story
As a product owner, I want a Grafana panel that graphs median route-replan latency for both regions so that I can quantify the 20–30 % speedup target.

## Acceptance Criteria
-   **AC-1:** A Grafana dashboard (or panel within a dashboard) shows p50 (median) and p95 latency for route replanning requests for the last 24 hours, comparing Wavelength Zone vs. standard Region.
-   **AC-2:** If Wavelength latency exceeds standard region latency for 3 consecutive minutes, an SNS alert fires.

## Key Components
-   **Metrics Collection:**
    -   The `RoutePlanner` Lambda (in both Wavelength and Region deployments) will need to emit custom metrics, likely to Amazon CloudWatch. These metrics should include:
        -   Request processing time (latency).
        -   An identifier for the processing region (e.g., "wavelength_us-wl1-bos-1", "region_us-east-1").
-   **Grafana Dashboard:**
    -   A Grafana instance (either self-managed or using Amazon Managed Grafana) will be configured.
    -   A data source connection to Amazon CloudWatch will be established.
    -   Panels will be created to:
        -   Graph p50 and p95 latencies over time (last 24 hours) for both Wavelength and Regional deployments side-by-side or overlaid.
        -   Potentially display other relevant metrics.
-   **Alerting:**
    -   CloudWatch Alarms will be configured based on the latency metrics.
    -   An alarm will be set to trigger if the Wavelength zone's p50 latency is greater than the standard region's p50 latency for a period of 3 consecutive minutes (e.g., 3 data points if metrics are emitted every minute).
    -   This CloudWatch Alarm will be configured to publish a notification to an Amazon SNS topic when it enters the ALARM state.
    -   Subscribers (e.g., email, SMS, or a Chime/Slack webhook via another Lambda) can be added to the SNS topic to receive these alerts.

## Implementation Notes
-   Ensure consistent tagging or dimensioning of CloudWatch metrics to easily distinguish between Wavelength and Regional data in Grafana and CloudWatch Alarms.
-   The 20-30% speedup target should be visually verifiable from the Grafana dashboard.
