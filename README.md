# habiTro ⚡

> An offline-first Habit & Routine Engineering Engine built with Kotlin, Jetpack Compose, and Clean Architecture.

habiTro is an opinionated, production-grade Android application designed to bridge high-performance systems engineering with modern declarative UI. Engineered to operate entirely offline, synchronize deterministically in the background, and provide instant, tactile micro-feedback for high-discipline daily execution.

---

## 💎 Signature Features

* **Cyber-Minimalist Heatmap Matrix:** High-density, GitHub-style habit frequency visualizer drawn directly via Compose `Canvas` (`drawRect`, `drawRoundRect`) to minimize layout thrashing and maintain 120 FPS scrolling.
* **Haptic Micro-Interactions:** Custom haptic feedback patterns tuned to hardware vibration actuators for completion clicks, streak unlocks, and destructive actions.
* **Sprint / Focus Protocol:** An AMOLED pure-black execution timer built using zero-overhead state hoisting and `derivedStateOf` to prevent redundant recompositions during high-frequency tick updates.
* **Habit Stacking Chains:** Group micro-habits into atomic workflows (e.g., *Morning Kickoff: Cold Shower → Read 10 Pages → Plan Day*), completing sequentially with one-tap handoffs.
* **Offline-First Sync Engine:** Room database serves as the Single Source of Truth (SSOT). Network interactions write to local cache first; background delta synchronization handles reconciliation via WorkManager with backoff retry policies.

---

## 🛠 Tech Stack & Architecture

### Core Architecture
* **Pattern:** MVVM + Clean Architecture with Unidirectional Data Flow (UDF)
* **Modularization:** 3-tier decoupling (`:app`, `:core`, `:feature`)
  * `:app` — Application initialization, dependency graphs, global navigation root
  * `:core:model` — Independent domain models and value objects
  * `:core:database` — Room entities, DAOs, TypeConverters, migrations
  * `:core:network` — Retrofit, OkHttp, Mock Engine, interceptors
  * `:core:ui` — Design tokens, themes, haptic drivers, Canvas primitives
  * `:feature:tracker` — Dashboard, habit list, completion toggles
  * `:feature:focus` — Deep-work sprint screen and countdown HUD
  * `:feature:editor` — Form validation, category pickers, `SavedStateHandle` integration

### Libraries & Frameworks
| Layer | Technologies |
| :--- | :--- |
| **Language** | Kotlin (100%), Coroutines, Structured Concurrency |
| **UI Framework** | Jetpack Compose (Material 3 tokens, Canvas APIs) |
| **Reactive Streams** | Kotlin `Flow`, `StateFlow`, `SharedFlow` |
| **Dependency Injection** | Dagger-Hilt (`@HiltAndroidApp`, `@ViewModelScoped`, `@Binds`) |
| **Local Persistence** | Room Database (SQLite, WAL mode, Flow-based queries) |
| **Pagination** | Paging 3 (`PagingSource`, `LazyPagingItems`) |
| **Networking** | Retrofit2, OkHttp3, Kotlinx Serialization |
| **Background Processing** | WorkManager (`CoroutineWorker`, constraints, exponential backoff) |
| **Navigation** | Navigation-Compose (SafeArgs / Type-Safe navigation contracts) |
| **Quality & Profiling** | JUnit5, MockK, Turbine, Compose UI Testing, LeakCanary, Baseline Profiles |

---

## ⚡ Feature Matrix

### 1. Habit Engine & Routine Management
* **Flexible Cadence:** Daily streaks, flexible targets (e.g., 3 days/week), and numerical increment tracking (e.g., Liters of water, Pages read).
* **Instant Inline Toggle:** Custom animated checkboxes with spring damping physics (`Animatable`).
* **Category Tagging:** Dynamic grouping with fast filtering via `derivedStateOf`.
* **State Preservation:** Add/Edit flows survive system process death via `SavedStateHandle`.

### 2. Analytics & Visual Heatmap
* **Commit Matrix:** 30/90/365-day density maps calculated off background threads.
* **Momentum & Velocity Score:** Algorithm-driven consistency index factoring in recovery rates after broken streaks.
* **Paging History Log:** Infinite-scroll event ledger driven by Paging 3.

### 3. Focus Protocol (Deep Work HUD)
* **AMOLED Optimized:** True `#000000` surface minimizing battery consumption on OLED displays.
* **Zero Jitter:** High-frequency timer decoupled from layout measure passes using draw-phase animations.

### 4. Background Sync & Reliability
* **Transactional Writes:** Mutations are staged in Room immediately; an asynchronous sync worker pushes deltas when constraints (network connection + battery not low) are satisfied.
* **Conflict Resolution:** Deterministic Last-Write-Wins (LWW) utilizing UTC epoch timestamps.

---

## 📂 Project Structure

```text
habiTro/
├── gradle/
│   └── libs.versions.toml             # Centralized Version Catalog
├── app/                               # App entry point, Application class, Navigation host
├── core/
│   ├── common/                        # Dispatcher providers, Result wrapper, Extensions
│   ├── database/                      # Room Database, Entities, DAOs, TypeConverters
│   ├── network/                       # Retrofit instance, Interceptors, Mock service
│   ├── designsystem/                  # Typography, Palette, Custom Canvas components
│   └── model/                         # Pure domain entities (Habit, Log, Metric)
├── domain/                            # Business logic layer
│   ├── repository/                    # Repository interfaces
│   └── usecase/                       # Single-purpose interactor classes
└── feature/
    ├── dashboard/                     # Main habit list, Heatmap, Card UI
    ├── focus/                         # Deep work timer screen
    ├── editor/                        # Habit creation/editing form
    └── analytics/                     # Detailed streak metrics and paging log

🧪 Testing & Quality Assurance
Unit Testing:

Domain UseCases tested against mocked/fake repository contracts using MockK and standard JUnit5.

Kotlin Coroutine Flow emissions asserted deterministically with Cash App's Turbine.

Database Integration:

Room DAOs validated against an in-memory SQLite database (Room.inMemoryDatabaseBuilder).

Compose UI Testing:

Screen interactions, state changes, and semantics assertions verified via createComposeRule() without Espresso dependencies.

Leak Detection:

Integrated LeakCanary for monitoring Activity Context leaks across configuration changes.

🚀 Build & Setup
Prerequisites
Android Studio Ladybug (2024.2.1+) or newer

JDK 17

Android SDK 35 (Minimum SDK: 26)

Installation
Bash
# Clone the repository
git clone [https://github.com/your-username/habiTro.git](https://github.com/your-username/habiTro.git)

# Navigate to project directory
cd habiTro

# Run unit tests across all modules
./gradlew testDebugUnitTest

# Assemble debug APK
./gradlew assembleDebug
📄 License
Plaintext
Copyright (c) 2026 habiTro contributors.
Distributed under the MIT License.
