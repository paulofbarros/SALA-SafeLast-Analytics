# SALA • SafeLast Analytics - System Architecture

This document describes the technical infrastructure and data flow of the SALA ecosystem, designed for biomechanical risk mitigation and fatigue monitoring in professional transport operations in Norway.

## 1. Tech Stack (The Engine)

SALA is designed to be lightweight, scalable, and cost-efficient:

* **Backend:** Python (Google Cloud Functions / AWS Lambda) - Serverless processing of the risk algorithm.
* **Database:** Firebase Realtime Database - For instant synchronization between the truck and the company portal.
* **Frontend:** HTML5, CSS3, JavaScript (Vanilla) - Optimized interfaces for low data consumption.
* **Data Integration:** MET Norway API (Weather) and Google Maps Platform (Telemetry).

## 2. Data Pipeline & Algorithm Flow

The **SALA Risk Index** calculation follows a 4-step process:

1.  **Ingestion:** The system receives telemetry data (Driving time, G-force/Accelerometer, GPS Location).
2.  **Enrichment:** The algorithm crosses the location with weather APIs to determine the *Surface Friction Coefficient* (Sf) in real-time (e.g., Snow vs. Dry asphalt).
3.  **Processing:** The Python engine processes the biomechanical formula:
    `SALA_Risk = (Fatigue_Level * 1.5) + (Surface_Risk * 1.2) + (Biomechanic_Load)`
4.  **Delivery:** The result is pushed via WebSockets to the Driver Dashboard and Company Portal in < 200ms.

## 3. Implementation Strategy (PoC)

To ensure accuracy and safety, the algorithm will be tested in three stages:

* **Phase A (Shadow Mode):** Back-testing with historical fleet data from Larvik to validate the algorithm's accuracy against real past incidents.
* **Phase B (Controlled Pilot):** Deployment in 5 vehicles operating on the E18 corridor (Southern route), collecting real-time data for calibration.
* **Phase C (Full Integration):** Launch of the BI portal for fleet managers and integration with HR systems for HMS (Health, Safety, and Environment) monitoring.

## 4. Security & Compliance (GDPR)

* **Anonymization:** Risk data is processed to protect the driver's identity, focusing on operational safety and biomechanical health.
* **Encryption:** All communication between the vehicle and the cloud uses TLS 1.3 protocols.
* **Compliance:** Fully aligned with the **Norwegian Working Environment Act (Arbeidsmiljøloven § 4-4)**.

---
**Author:** Paulo Fernando de Barros  
**Status:** Architecture Design Phase (Ready for PoC)
