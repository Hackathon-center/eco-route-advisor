# Project Title: Fleet Management and Optimisation System

## Overview
This project aims to develop a comprehensive fleet management and optimisation system. It includes features for simulating fleet operations, optimizing routes for fuel efficiency, providing real-time updates to drivers and fleet managers, generating eco-driving tips, and ensuring data security and auditability.

## Epics

The project is divided into the following epics:

1.  **Ingestion**: Handles the initial intake of GPS data.
    *   *User Story*: As a DevOps engineer, I want to upload a CSV file containing pre-recorded GPS pings so that we can simulate a fleet without hardware.
2.  **QA Tools**: Provides tools for quality assurance and demo purposes.
    *   *User Story*: As a QA tester, I want a Postman collection that seeds sample files so that any team-mate can spin up a demo in < 5 min.
3.  **Optimisation Engine**: Focuses on route optimisation.
    *   *User Story*: As the optimisation engine, I want to call SageMaker Geospatial every 60 s to recompute shortest paths that avoid congestion so that fuel burn is minimised.
4.  **Driver Mobile App**: Delivers route information and tips to drivers.
    *   *User Story*: As a driver, I want my mobile view to highlight the updated route in ≤ 5 s after optimisation so that I can adjust without confusion.
5.  **Sustainability Analytics**: Generates eco-friendly driving advice.
    *   *User Story*: As a sustainability analyst, I want Bedrock to generate bite-sized eco-tips based on driving behaviour so that drivers learn how to save fuel.
6.  **Fleet Manager Dashboard**: Provides a web interface for fleet overview and tip logging.
    *   *User Story 1*: As a fleet manager, I want the tips logged against each trip so that I can review driver coaching history.
    *   *User Story 2*: As a dispatcher, I want a web dashboard that shows current vehicle positions, planned routes and litres saved so that I see ROI in real time.
7.  **DevOps**: Manages infrastructure and deployment.
    *   *User Story*: As a DevOps engineer, I want to deploy the optimisation Lambda in both a Wavelength Zone (us-wavelength-1) and a standard region (us-east-1) so that we can A/B test latency.
8.  **Reporting**: Focuses on performance monitoring and alerting.
    *   *User Story*: As a product owner, I want a Grafana panel that graphs median route-replan latency for both regions so that I can quantify the 20–30 % speedup target.
9.  **Demo Scripts**: Contains scripts for showcasing specific features.
    *   *User Story*: As a judge, I want a demo script that throttles bandwidth to 1 Mbps and shows Wavelength still hitting < 1 s replans so that the telco value prop is obvious.
10. **Security**: Ensures data protection.
    *   *User Story*: As a security officer, I want all location data encrypted at rest with AWS-managed KMS keys so that we meet basic compliance.
11. **Audit**: Manages access logging and review.
    *   *User Story*: As a privacy-conscious fleet manager, I want to view audit logs of who accessed trip data so that I can investigate incidents.

Further details for each epic can be found in their respective directories.
