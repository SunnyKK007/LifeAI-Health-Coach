# Technical Stack & Engineering Skills

This document details the exact technical competencies and frameworks used to construct LifeAI, categorized by their deployment phase.

## 1. Phase 1: Frontend Architecture & Edge Compute
*   **Framework:** React Native (TypeScript ecosystem).
*   **Execution Paradigm:** Full deployment of the React Native **New Architecture**. 
    *   **JSI (JavaScript Interface):** Eliminating the legacy JSON bridge. Custom C++ native bindings provide direct, synchronous memory-level access to host system primitives.
    *   **Fabric Renderer:** Enables synchronous UI operations and direct integration with React 18 Concurrent features to achieve zero visual layout latency.
*   **Computer Vision Layer:** Decomposing real-time camera viewfinder inputs via **TensorFlow.js** utilizing WebGL backends for hardware acceleration on device GPUs.
*   **Edge LLM Runtime:** Meta's **ExecuTorch** compiled directly into the application via `react-native-executorch`. Hosts quantized 4-bit language models (e.g., Phi-3-mini) for localized RAG chat without cloud dependencies.
*   **Baseline Telemetry:** Implementation of native Apple HealthKit and Android Google Health Connect SDKs.

## 2. Phase 1: Asynchronous Backend Infrastructure
*   **API Runtime Engine:** **FastAPI (Python 3.11+)** engineered for fully asynchronous request handling using Uvicorn workers.
*   **Relational Engine:** PostgreSQL running via Amazon RDS with Multi-AZ replication to store core user credentials and clinical roadmaps.
*   **Telemetry Cache:** Redis clusters deployed to cache rapid access to token maps required by localized RAG inference workers.

## 3. Phase 2: Advanced Integrations & Data Engineering
*   **Medical Hardware SDKs:** Advanced integration of Bluetooth Low Energy (BLE) protocols and external healthcare partner APIs (Dexcom, Abbott, Omron) to securely stream clinical vitals.
*   **Predictive Analytics Runtime:** **Scikit-learn and XGBoost** implementations running inside Python microservices.
    *   **Pipeline Engineering:** Feature engineering targeting behavioral matrices (app session length, telemetry frequency) to generate predictive churn and regression Risk Scores.
*   **Enterprise Dashboards:** React.js web portals heavily utilizing secure WebSockets for real-time coach-to-patient communication. 
*   **Data Aggregation:** Deploying analytical scripts to sanitize and aggregate population-level data structures for B2B HR reporting without violating PII constraints.  
