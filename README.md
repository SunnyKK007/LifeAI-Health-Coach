# 🧬 LifeAI Health Coach

> **A unified, AI-powered digital health ecosystem utilizing real-time data synthesis to eliminate fragmented health tracking.**

![React Native](https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![OpenAI](https://img.shields.io/badge/GPT--4o-412991?style=for-the-badge&logo=openai&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)

## 📌 The Problem: The Data Silo Bottleneck
Modern digital health relies on isolated systems—users track nutrition in one app, steps in another, and receive coaching somewhere else. 
1. **Context Disconnect:** A sudden drop in a user's health score lacks the context of their dietary habits or sleep cycles.
2. **Cognitive Friction:** Manual logging across multiple interfaces drives high user churn.
3. **Reactive Systems:** Existing apps act as historical ledgers rather than proactive, personalized agents.

## 🧠 The Solution: Unified Intelligence Flow
LifeAI acts as a centralized intelligence engine. By synthesizing wearable telemetry, visual food scanning, and RAG-based conversational AI, the platform provides proactive, context-aware health interventions in a single interface.

## 🏗️ Technical Architecture & Stack

To handle multimodal data efficiently, the backend utilizes a robust microservices and multi-database architecture:

| Domain | Technologies | Purpose |
| :--- | :--- | :--- |
| **Mobile Core** | React Native, TypeScript | Single codebase for native iOS and Android deployment. |
| **Backend API** | FastAPI (Python 3.11+) | High-speed, asynchronous request handling and WebSockets. |
| **Database Structure** | PostgreSQL, AWS S3 | Relational user data, secure authentication, and image storage. |
| **AI & Memory (RAG)**| GPT-4o, Pinecone / pgvector | Conversational coaching with persistent vector memory context. |
| **Computer Vision** | LogMeal API | Real-time spatial food recognition and macronutrient extraction. |
| **Telemetry Sync** | Apple HealthKit, Google Health Connect | Background ingestion of steps, sleep, and active energy. |

## 🚀 Execution Roadmap

### Phase 1: Core MVP (40-Day Sprint)
*   **AI Health Coach:** 24/7 personalized conversational agent utilizing a RAG architecture to map advice against the user's specific 90-day roadmap.
*   **CV Food Scanner:** Cloud-based visual volumetric and macronutrient estimation (Protein, Carbs, Fats) via API.
*   **Wearable Syncing:** Silent, battery-efficient background syncing of step counts and sleep cycles natively from the OS.
*   **Habit & Coach Dashboards:** Daily task tracking loops for users and a React-based B2B web console for human clinicians to monitor progress.

### Phase 2: Medical & Predictive Expansion (Post-MVP)
*   **Predictive ML Risk Engine:** XGBoost classification models utilizing behavioral metadata to forecast user churn and regressions.
*   **Clinical Hardware Sync:** Direct BLE integrations for Continuous Glucose Monitors (CGM) and smart blood pressure cuffs.
*   **Enterprise B2B Portals:** Anonymized population health analytics for corporate HR departments.

## 💻 Local Development Setup

Because this is a React Native project, a single TypeScript codebase deploys natively to both iOS and Android. 

### Prerequisites
*   **Node.js (v18+):** Required to run the Metro Bundler (the local development server that compiles JavaScript) and manage `npm` packages. *(Note: The backend API runs independently on Python/FastAPI).*
*   **Android Studio:** Provides the Android SDK and Emulator for local Android testing.
*   **Xcode (Mac only):** Provides the iOS Simulator and CocoaPods for linking native iOS libraries.

### Installation
1. Clone the repository and install the JavaScript dependencies:
```bash
   git clone [https://github.com/yourusername/LifeAI-Health-Coach.git](https://github.com/yourusername/LifeAI-Health-Coach.git)
   cd LifeAI-Health-Coach
   npm install
