<div align="center">
  <img src="screenshots/questlog_logo_scaled.png" alt="QuestLog Core" height="150" style="filter: drop-shadow(0 0 15px rgba(0, 0, 0, 0.3));"/>
  
  <h1 style="font-family: 'Roboto', sans-serif; font-weight: 300; letter-spacing: 3px; text-transform: uppercase;">QUESTLOG</h1>
  <h3 style="color: #666; font-weight: 400; letter-spacing: 1px;">Gamified Productivity & Social RPG Platform for Android</h3>

  <p>
    <img src="https://img.shields.io/badge/Platform-Android_Native_API_33-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android API 33"/>
    <img src="https://img.shields.io/badge/Language-Java_17_LTS-007396?style=for-the-badge&logo=java&logoColor=white" alt="Java 17"/>
    <img src="https://img.shields.io/badge/Backend-Firebase_Realtime_%26_Firestore-FFCA28?style=for-the-badge&logo=firebase&logoColor=white" alt="Firebase"/>
    <img src="https://img.shields.io/badge/Persistence-Hybrid_(SQLite_%2B_Cloud)-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="Hybrid DB"/>
    <img src="https://img.shields.io/badge/System-Actionable_Notifications_FCM-FF5722?style=for-the-badge&logo=googlemessages&logoColor=white" alt="Actionable Notifications"/>
    <img src="https://img.shields.io/badge/Logic-Sensor_Fusion_%26_Algorithms-8E24AA?style=for-the-badge&logo=reactos&logoColor=white" alt="Sensor Fusion"/>
  </p>

  <p align="center" style="max-width: 900px; margin: auto; line-height: 1.8; font-size: 1.1em;">
    <strong>QuestLog is a meticulously engineered native Android application, pushing the boundaries of mobile productivity by integrating sophisticated MMORPG mechanics. 
    It establishes an "Offline-First" distributed data architecture, features a robust probabilistic combat engine, and leverages advanced Android OS capabilities for real-time, actionable, and context-aware user engagement.</strong>
  </p>

  <p style="margin-top: 20px;">
    <a href="https://raw.githubusercontent.com/bteodora/e2-ma-tim04-2025/main/screenshots/Specifikacija%20projekta.pdf"><strong>📄 View Engineering Specification (PDF) »</strong></a>
    <span style="padding: 0 15px">|</span>
    <a href="#architecture-and-data-flow"><strong>🏛 Explore Architecture »</strong></a>
    <span style="padding: 0 15px">|</span>
    <a href="#engineering-deep-dive"><strong>⚙️ Engineering Deep Dive »</strong></a>
  </p>
</div>

<br>

---

## 1. Executive Summary

**QuestLog** redefines habit tracking by transforming mundane daily tasks into an engaging, progressive RPG narrative. This platform addresses critical shortcomings in user engagement by implementing a **complex Gamification Metagame** (algorithmic progression, itemization, boss encounters) coupled with a **Real-time Social Ecosystem** (Alliances, cooperative missions).

From a software engineering standpoint, QuestLog is a comprehensive demonstration of **Android SDK mastery**. It transcends typical mobile applications by featuring:
*   **Offline-First Data Architecture:** Ensuring seamless user experience regardless of network connectivity.
*   **Advanced System Integration:** Leveraging background services, foreground services, and hardware sensors for context-aware interactions.
*   **Robust Algorithmic Core:** Implementing deterministic and probabilistic logic for character progression, combat, and reward distribution.
*   **Bi-directional Communication:** Pioneering actionable, real-time notifications for critical social and gameplay events.

---

## 2. Architecture and Data Flow

The application is structured around a **Layered Architecture** (Presentation, Business Logic, Data Access) within an **Offline-First** paradigm. This ensures high responsiveness and data integrity, even during network outages.

### 2.1 Hybrid Persistence Strategy
To achieve both offline capability and real-time cloud synchronization, QuestLog employs a sophisticated **Hybrid Persistence** model:
*   **Local Data Store (SQLite):** Serves as the primary, high-performance local cache. All mission-critical user data (task lists, XP, HP, inventory) is stored in a normalized relational schema. This guarantees **zero-latency UI updates** and uninterrupted gameplay.
*   **Cloud Data Store (Firebase Firestore / Realtime Database):** Firebase acts as the central, authoritative source of truth. It stores user profiles, social graphs (friends, alliances), and shared mission states. Background services orchestrate **asynchronous, eventually-consistent synchronization** between SQLite and Firebase, implementing conflict resolution strategies.

### 2.2 Background Synchronization & Event-Driven Updates
Maintaining data consistency and delivering real-time social interactions requires robust background processing.
*   **Firebase Cloud Messaging (FCM):** The backbone for push notifications. FCM payloads trigger specific `BroadcastReceivers` and `JobIntentServices` to process updates in the background, even when the app is not active.
*   **Real-time Database Listeners:** `ValueEventListeners` on Firebase Realtime Database are meticulously managed to subscribe to critical social events (e.g., Alliance Chat messages, Boss HP changes), ensuring instant, low-latency updates.
*   **WorkManager/AlarmManager (Implicit):** Implicitly, complex, time-bound tasks (like long-running Alliance missions) would leverage `WorkManager` for guaranteed execution or `AlarmManager` for precise scheduling, ensuring integrity of the game state.

---

## 3. Engineering Deep Dive

### 3.1 Advanced Notification System
QuestLog redefines mobile user engagement through highly interactive and context-aware notifications.
*   **Actionable Intents & Remote Input:** Notifications are not passive. Users can **reply to Alliance chat messages** or **accept/decline Alliance invitations** directly from the notification shade using `RemoteInput`. This minimizes friction and enhances social immediacy.
*   **Deep Linking & Back Stack Management:** Clicking a complex notification (e.g., "Alliance Boss Raid has started!") navigates the user directly to the relevant `Fragment` within the application, constructing a synthetic back stack to preserve navigational context.
*   **Notification Channels:** Adheres to Android's `NotificationChannel` API for granular user control over notification categories and priorities, enhancing user experience and compliance.

### 3.2 Deterministic & Probabilistic Game Logic
The core gamification engine is a blend of mathematical precision and calculated risk.
*   **Algorithmic Progression:** User leveling and Boss HP scaling follow complex, dynamically calculated formulas based on current level and historical performance. This ensures a balanced and challenging progression curve.
*   **Probabilistic Combat Engine:** Boss battles incorporate a sophisticated probabilistic algorithm where the "Hit Chance" is dynamically calculated based on the user's real-world task completion success rate over a defined period (e.g., last 7 days). This ties real-world habits directly to in-game success.
*   **Deterministic Formulas:** XP allocation, currency rewards, and item drop rates are governed by meticulously defined deterministic formulas, ensuring fairness and predictability within the game economy.

### 3.3 Sensor Fusion & Physical Interaction
QuestLog integrates directly with device hardware to create immersive interactions.
*   **Accelerometer Integration:** Utilizes the `SensorManager API` to listen for raw accelerometer data. Custom signal processing logic detects specific "Shake" events, transforming physical user movement into impactful in-game combat actions (e.g., attacking a Boss).
*   **QR Code Integration:** Seamlessly integrates `ZXing` for generating and scanning QR codes, facilitating effortless friend discovery and Alliance invites without manual input.

---

## 4. Functional Modules & Implementation Details

| Module | Technical Implementation Highlights |
| :--- | :--- |
| **Identity & Session Management** | **Secure Auth:** Firebase Authentication (Email/Password) for robust user identity. Local session persistence using `SharedPreferences` with custom encryption for sensitive user settings. |
| **User Statistics & Analytics** | **Dynamic Visualization:** `MPAndroidChart` integration to render real-time, interactive graphs (line, bar, donut charts) displaying progress metrics (XP velocity, task categories, success streaks) directly from optimized SQLite queries. |
| **Social Ecosystem & Alliance Raids** | **Real-Time Presence:** Firebase Realtime Database manages Alliance membership and chat. **Cooperative Boss Mechanics:** Alliance-wide Boss HP scales with member count, requiring synchronized task completion to defeat. |
| **Virtual Economy & Inventory** | **Transactional Integrity:** Manages in-game currency (Gold) and equipment acquisition/utilization. Complex logic for item activation, temporary/permanent buffs, and weapon upgrades with statistical modifiers. |
| **Task Management & Scheduling** | **Flexible Task Lifecycle:** Supports single-shot and recurring tasks with granular scheduling. Manages task states (Active, Completed, Paused, Canceled, Failed) with time-sensitive auto-transitions. |

---

## 5. Technology Stack

### Core Android Development
*   **Language:** Java 17 (leveraging modern language features)
*   **Framework:** Android SDK (API Level 33+)
*   **Architecture:** Adheres to a strict Layered Architecture (Presentation, Business Logic, Data Access) within the Android component model (Activities, Fragments).
*   **Concurrency:** Robust use of `ExecutorService`, `HandlerThread`, and `AsyncTask` for efficient background task execution, ensuring UI responsiveness.

### Backend-as-a-Service (BaaS) & Cloud Integration
*   **Authentication:** Firebase Authentication (secure, scalable user identity management).
*   **Databases:**
    *   **Cloud Firestore:** Primary cloud store for user profiles, items, and structured game data.
    *   **Firebase Realtime Database:** Optimized for low-latency, real-time social interactions (chat, live Boss HP).
*   **Messaging:** Firebase Cloud Messaging (FCM) for push notifications (unicast and topic-based).

### Advanced Libraries & Integrations
*   **Data Visualization:** `MPAndroidChart` for high-performance, interactive graph rendering.
*   **Animations:** `Lottie` for fluid, JSON-based vector animations, enhancing UX feedback (e.g., loot box opens, boss hit effects).
*   **QR Codes:** `ZXing QR Code Scanner` for seamless camera integration and QR code functionality.
*   **UI/UX:** `Material Design Components` for a consistent, modern visual language and `DayNight` themes for adaptive UI.

---

## 6. UI/UX Gallery

The application implements a meticulously crafted UI/UX, featuring full adaptive theming with `DayNight` resources to automatically switch between light and dark modes based on system preferences.

### Authentication & Core Profile
| Secure Registration | Seamless Login | Dynamic User Profile |
|:---:|:---:|:---:|
| <picture><source srcset="screenshots/Register_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Register_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Register_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/Login_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Login_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Login_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/Profile_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Profile_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Profile_light.jpg" width="220"></picture> |

### Alliance & Social Interaction
| Friend Discovery | Alliance Hub | Real-time Chat |
|:---:|:---:|:---:|
| <picture><source srcset="screenshots/Friends_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Friends_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Friends_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/AllianceInfo_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/AllianceInfo_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/AllianceInfo_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/AllianceChat_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/AllianceChat_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/AllianceChat_light.jpg" width="200"></picture> |

### Inventory & In-App Economy
| Virtual Shop | Active Inventory | User Statistics |
|:---:|:---:|:---:|
| <picture><source srcset="screenshots/Shop_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Shop_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Shop_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/Equipped_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Equipped_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Equipped_light.jpg" width="220"></picture> | <picture><source srcset="screenshots/Stats1_dark.jpg" media="(prefers-color-scheme: dark)"><source srcset="screenshots/Stats1_light.jpg" media="(prefers-color-scheme: light)"><img src="screenshots/Stats1_light.jpg" width="220"></picture> |

---

## 7. Demo & Engineering Specification

<p align="left" id="demo-preview">
  <a href="https://github.com/user-attachments/assets/9473a89a-8e74-4167-9904-aef7dd8c234f">
    <img src="screenshots/thumbnail.png" alt="Demo Video" width="100%" style="border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.2);"/>
  </a>
</p>

<p align="center" style="margin-top: 20px;">
  <a href="https://raw.githubusercontent.com/bteodora/e2-ma-tim04-2025/main/screenshots/Specifikacija%20projekta.pdf" target="_blank">
    <img src="https://img.shields.io/badge/View_Full_Engineering_Specification-ACB2E2?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="Full Engineering Specification"/>
  </a>
</p>
