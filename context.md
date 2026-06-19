# Project Context: LifeAI Health Coach

## 1. Product Paradigm & Vision
LifeAI Health Coach is an AI-first digital health ecosystem designed to unify fragmented preventative care management. Instead of forcing users to navigate isolated applications for nutrition logging, activity tracking, and coaching, LifeAI utilizes on-device edge computing to act as a centralized intelligence layer. 

## 2. Problem Statement: The Data Silo Bottleneck
Modern digital health infrastructure suffers from severe fragmentation. 
1. **Asynchronous Context Disconnect:** A glucose spike logged in a medical app lacks the immediate context of the carbohydrates logged inside a nutritional app.
2. **Cognitive Ingestion Friction:** Manual logging across multiple interfaces drives high user burnout.
3. **Reactive Interventions:** Existing solutions function as historical ledgers rather than utilizing real-time predictive machine learning to actively intercept behavioral regressions.

## 3. The Centralized Edge Intelligence Flow
LifeAI eliminates the asynchronous data bottleneck by utilizing client-side edge machine learning (JavaScript Inference) paired with a modern asynchronous microservices backend.

    [User captures Photo Input]
             │
             ▼
    ┌────────────────────────────────────────────────────────┐
    │ On-Device Computer Vision (TensorFlow.js / WebGL)      │
    │ - Instant Macronutrient & Volumetric Estimation        │
    └────────────────────────┬───────────────────────────────┘
                             │
                             ▼
    ┌────────────────────────────────────────────────────────┐
    │ Synchronous JSI Telemetry Layer                        │
    │ - Pulls concurrent activity & baseline telemetry       │
    └────────────────────────┬───────────────────────────────┘
                             │
                             ▼
    ┌────────────────────────────────────────────────────────┐
    │ Local Edge LLM (ExecuTorch Runtime Engine)             │
    │ - Runs localized RAG pipeline against clinical roadmap │
    └────────────────────────┬───────────────────────────────┘
                             │
                             ▼
    ┌────────────────────────────────────────────────────────┐
    │ Output: Immediate, personalized, contextual feedback   │
    └────────────────────────────────────────────────────────┘

## 4. Phased Product Modules

### Phase 1: Core Ecosystem (Consumer MVP)
The immediate MVP focuses entirely on establishing baseline intelligence and eliminating daily logging friction.
*   **User Onboarding & Assessment:** Dynamic generation of an initial Health Score and algorithmically generated 90-day lifestyle roadmap.
*   **AI Health Coach (RAG Architecture):** A 24/7 hyper-personalized conversational companion running locally via ExecuTorch to ensure zero latency and maximum privacy.
*   **AI Nutrition & Food Scanner:** Edge computer vision via TensorFlow.js that detects and estimates macronutrients directly from the mobile camera viewfinder.
*   **Baseline Activity Sync:** Background ingestion of steps, active kilocalories, and sleep duration via Apple HealthKit and Google Health Connect.
*   **Human Coach Safety Net:** A foundational B2B operations dashboard allowing human clinicians to monitor flagged user anomalies and communicate via secure WebSockets.

### Phase 2: Advanced Medical, Predictive & Enterprise Expansion
The secondary phase scales the MVP into a clinical-grade preventative healthcare platform.
*   **Diabetes Module (CGM Sync):** Direct API integration with Dexcom and Abbott Continuous Glucose Monitors, mapping blood sugar spikes directly against the photo food logs to identify personalized trigger foods.
*   **Cardiology Module:** Bluetooth Low Energy (BLE) integration with smart blood pressure cuffs (e.g., Omron) and smart scales, triggering automatic alerts to human coaches if vitals enter hypertensive ranges.
*   **Predictive Machine Learning (Proactive Care):** XGBoost classification models that analyze behavioral metadata (time spent in app, typing speed, logging frequency). Models flag user profiles with a "Risk Score" to intercept burnout before churn occurs.
*   **Corporate Wellness Dashboard:** A B2B portal for HR departments displaying strictly anonymized aggregate data regarding employee engagement, weight lost, and estimated healthcare dollars saved.
*   **Social Challenges:** Community leaderboards enabling company-wide step challenges and peer accountability loops.
