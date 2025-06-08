# EcoRoute Advisor 🚚🌱

**EcoRoute Advisor** is a virtual driving range for experimenting with eco-friendly routing ideas—no real trucks, SIM cards, or GPS devices required.

We replay a CSV of timestamped GPS points into AWS, simulating a live fleet. With telco-edge compute, AI coaching, and real-time dashboards, EcoRoute Advisor showcases the power of AWS for sustainability and latency-sensitive workloads.

---

## 📡 Architecture Overview

1. **CSV Replay**  
   A Lambda function replays `vehicles.csv` and streams fake GPS positions to Amazon Location Service (Tracker).

2. **Route Calculation (Region vs Edge)**  
   Another Lambda (`RoutePlanner`) runs in:
   - A standard AWS Region (`us-east-1`)
   - An AWS Wavelength Zone (inside a 5G telco)

   Both use **SageMaker Geospatial APIs** to get fuel-efficient routes.

3. **AI Coaching**  
   Driving stats (idle time, speed variance) are sent to a **Bedrock InlineAgent** for natural language eco-driving tips.

4. **Real-Time Fan-out**  
   New data (locations, tips, routes) is saved to **DynamoDB** and broadcast via **AWS AppSync** (GraphQL Subscriptions).

5. **Visualization**  
   A **Next.js + MapLibre** frontend shows:
   - Moving truck icons
   - Eco (green) vs Original (purple) routes
   - Fuel and CO₂ counters
   - Edge vs Region replan latency chart

---

## 🧩 AWS Components

| Service                     | Role                                                                 |
|----------------------------|----------------------------------------------------------------------|
| Amazon Location Service     | Tracker + map tiles; logs all GPS points.                            |
| AWS Wavelength              | Runs RoutePlanner at telco edge for low-latency testing.             |
| SageMaker Geospatial        | Finds fuel-efficient routes using satellite data.                    |
| Amazon Bedrock (InlineAgent)| Generates plain-English eco-driving tips.                            |
| AWS AppSync + DynamoDB      | Real-time GraphQL + persistent state storage.                        |
| Next.js (frontend)          | Delivers dispatcher and driver views from one codebase.              |

---

## 🧪 Construction Stages

| Stage         | What You See on the Dashboard                                  |
|---------------|---------------------------------------------------------------|
| `S-1`         | Dots moving on roads from CSV replay                          |
| `S-2`         | Green route appears + "Litres saved" counter                  |
| `S-3`         | Latency widget shows Region vs Edge times                     |
| `S-4`         | AI "eco-tips" pop up beside trucks                           |
| `S-5`         | Full dispatcher dashboard: vehicles, charts, map              |
| `S-6`         | Edge latency demo harness for judges                          |

---

## 📈 Why It Matters

- Transforms offline CSVs into real-time telemetry
- Showcases real-world 5G + edge compute with **Wavelength**
- Demonstrates **AI for Sustainability** in motion
- Ideal for hackathons, AWS demos, or fleet simulations

---

## 🚀 Quick Start (coming soon)
TBD: Setup scripts, sample CSV, and deploy instructions.
