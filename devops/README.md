# DevOps & Infrastructure

## Purpose
This section outlines the infrastructure setup and deployment strategy, particularly focusing on the deployment of the RoutePlanner Lambda function to both AWS Wavelength Zones and standard AWS Regions for A/B testing latency and performance.

## User Story
As a DevOps engineer, I want to deploy the optimisation Lambda (RoutePlanner) in both a Wavelength Zone (e.g., `us-wavelength-1`) and a standard region (e.g., `us-east-1`) so that we can A/B test latency.

## Acceptance Criteria
-   **AC-1:** `cdk deploy --context env=wavelength` provisions the edge stack successfully. (This implies the use of AWS CDK for infrastructure as code).
-   **AC-2:** The RoutePlanner API (when invoked, regardless of where it's deployed) returns a header `X-Region-Id` (or a similar custom header) to indicate which path (Wavelength or standard Region) served the request.

## Key Components & Strategy
-   **Infrastructure as Code (IaC):**
    -   AWS Cloud Development Kit (CDK) will be used to define and provision cloud resources.
    -   Separate CDK stacks (or configurations within a stack) will be defined for:
        -   The standard regional deployment (e.g., `us-east-1`).
        -   The Wavelength Zone deployment (e.g., `us-wavelength-1-wl1-bos-wlz-1`).
-   **RoutePlanner Lambda Deployment:**
    -   The same Lambda function code for `RoutePlanner` will be deployed to both environments.
    -   The Lambda will be configured to identify its execution environment (Region vs. Wavelength Zone) and include this information in its response (e.g., via the `X-Region-Id` header).
-   **Deployment Process:**
    -   The `cdk deploy` command with appropriate context parameters (like `--context env=wavelength` or `--context env=region`) will be used to target specific environments.
-   **API Gateway:**
    -   Each RoutePlanner Lambda (one in the Region, one in Wavelength) will likely be fronted by its own API Gateway endpoint.
    -   The application logic (perhaps in the Fleet Manager Dashboard or a dedicated testing tool) will be responsible for calling both endpoints to compare latencies.
-   **Configuration:**
    -   Environment-specific configurations (e.g., SageMaker Geospatial endpoint configurations if they differ, though ideally they wouldn't for the same AWS region) will be managed through Lambda environment variables or CDK parameters.

## Considerations for Wavelength Zones
-   Wavelength Zones are extensions of an AWS Region into telecommunication providers' 5G networks.
-   Not all AWS services are available in Wavelength Zones. The `RoutePlanner` Lambda's dependencies (e.g., SageMaker Geospatial SDK access) need to be compatible.
-   Network paths and latency are key considerations. The goal is to demonstrate lower latency for requests originating from 5G devices when served by the Wavelength Zone.
