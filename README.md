# habiTro ⚡

> **Privacy-first, offline-first habit and routine tracking for Android.**

habiTro is a modern Android habit and routine tracking application built with **Kotlin, Jetpack Compose, Room, Hilt, Coroutines, and Google Drive AppData**.

The application follows an **offline-first architecture**, where the local Room database acts as the primary source of truth. Users can create habits, track routines, monitor streaks, analyze consistency, and run focused work sessions without requiring an internet connection.

For backup and device migration, habiTro optionally uses the user's private **Google Drive `appDataFolder`** instead of relying on a dedicated application backend.

---

## ✨ Features

### 🗓️ Habit & Routine Tracking

* Create, edit, and delete habits
* Daily and weekly schedules
* Habit categories
* Streak tracking
* Completion history
* Quantified habits
* Custom targets and units
* Habit filtering
* Atomic habit chains

Example:

```text
Hydrate
   ↓
Mobility
   ↓
Reading
   ↓
Deep Work
```

---

### 📊 Analytics & Activity Heatmap

Track consistency through a GitHub-style activity heatmap.

* 30-day activity view
* 90-day activity view
* 365-day activity view
* Streak statistics
* Completion history
* Recovery tracking
* Historical activity logs
* Paginated history using Paging 3

The heatmap is rendered using Jetpack Compose `Canvas` primitives to keep rendering lightweight.

---

### ☁️ Google Drive Backup

habiTro uses Google's **Drive `appDataFolder`** for application-specific backup data.

```text
Android Device
      │
      ▼
Room Database
      │
      │ Backup
      ▼
Google Drive
 appDataFolder
```

Backup functionality includes:

* Manual backup
* Restore
* Background backup
* Backup metadata
* Schema version validation
* Device migration support
* No custom application backend required

> The Room database remains the primary source of truth. Google Drive is used as a backup and synchronization mechanism.

---

### ⏱️ Sprint Protocol

A distraction-free focus timer designed for deep-work sessions.

* Countdown timer
* Start / pause / resume
* Session completion tracking
* Minimal interface
* AMOLED-friendly dark UI
* Optimized Compose state management

---

### 📳 Haptic Feedback

Optional tactile feedback for important interactions:

* Habit completion
* Streak milestones
* Reset actions
* Focus timer interactions

---

# 🏗️ Architecture

habiTro follows:

* **MVVM**
* **Clean Architecture**
* **Unidirectional Data Flow (UDF)**
* **Repository Pattern**
* **Offline-first data architecture**

```text
┌─────────────────────────────────────┐
│          Presentation Layer         │
│                                     │
│       Jetpack Compose UI            │
│                │                    │
│                ▼                    │
│            ViewModel                │
│                │                    │
│        StateFlow / UiState          │
└────────────────┬────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│             Domain Layer            │
│                                     │
│              UseCases               │
│                                     │
│       Repository Interfaces         │
└────────────────┬────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│              Data Layer             │
│                                     │
│      Repository Implementations     │
│                │                    │
│       ┌────────┴─────────┐          │
│       ▼                  ▼          │
│   Room Database      Drive Sync     │
│   Local Storage      Backup Layer   │
└─────────────────────────────────────┘
```

---

# 🔄 Data Flow

A typical habit completion follows this flow:

```text
User taps "Complete"
        │
        ▼
Compose Screen
        │
        ▼
ViewModel
        │
        ▼
CompleteHabitUseCase
        │
        ▼
HabitRepository
        │
        ▼
Room DAO
        │
        ▼
SQLite Database
        │
        ▼
Flow emits updated data
        │
        ▼
ViewModel StateFlow
        │
        ▼
Compose UI
```

The UI never directly interacts with the database.

This keeps presentation, business logic, and data access separated and testable.

---

# 📦 Modularization

The project uses a multi-module architecture to keep application features and infrastructure decoupled.

```text
habiTro
│
├── app
│
├── core
│   ├── common
│   ├── model
│   ├── database
│   ├── sync
│   └── designsystem
│
├── domain
│   ├── repository
│   └── usecase
│
└── feature
    ├── dashboard
    ├── focus
    ├── editor
    ├── analytics
    └── backup
```

---

## `:app`

Application entry point.

Responsibilities:

* Application class
* Hilt initialization
* Navigation host
* Application-level configuration
* Module integration

---

## `:core:model`

Contains pure Kotlin domain models and value objects.

Example:

```kotlin
data class Habit(
    val id: String,
    val name: String,
    val categoryId: String?,
    val target: Int?,
    val unit: String?,
    val isArchived: Boolean
)
```

---

## `:core:database`

Responsible for local persistence.

Contains:

* Room Database
* Entities
* DAOs
* TypeConverters
* Database migrations
* Local queries

```text
RoomDatabase
     │
     ├── HabitDao
     ├── HabitLogDao
     ├── CategoryDao
     └── ...
```

---

## `:core:sync`

Responsible for Google Drive backup and restore.

Contains:

* Google authentication
* Drive API integration
* Backup creation
* Restore logic
* Backup metadata
* Schema/version validation

---

## `:core:designsystem`

Contains reusable UI components and design tokens.

* Typography
* Colors
* Dimensions
* Shapes
* Compose components
* Canvas components
* Haptic utilities

---

## `:domain`

Contains application business logic.

```text
domain/
├── repository/
│   ├── HabitRepository.kt
│   └── SyncRepository.kt
│
└── usecase/
    ├── CreateHabitUseCase.kt
    ├── CompleteHabitUseCase.kt
    ├── GetHabitsUseCase.kt
    ├── BackupToDriveUseCase.kt
    └── RestoreFromDriveUseCase.kt
```

The domain layer does not depend on Android UI components.

---

## `:feature`

Contains independent application features.

```text
feature/
├── dashboard/
├── focus/
├── editor/
├── analytics/
└── backup/
```

A feature can follow a structure such as:

```text
feature/
└── dashboard/
    ├── DashboardScreen.kt
    ├── DashboardViewModel.kt
    ├── DashboardUiState.kt
    └── ...
```

---

# 🛠️ Tech Stack

| Category              | Technology                      |
| --------------------- | ------------------------------- |
| Language              | Kotlin                          |
| UI                    | Jetpack Compose                 |
| Design System         | Material 3                      |
| Architecture          | MVVM + Clean Architecture + UDF |
| Database              | Room / SQLite                   |
| Async Programming     | Kotlin Coroutines               |
| Reactive State        | Flow / StateFlow / SharedFlow   |
| Dependency Injection  | Hilt                            |
| Navigation            | Navigation Compose              |
| Pagination            | Paging 3                        |
| Background Processing | WorkManager                     |
| Cloud Backup          | Google Drive API v3             |
| Authentication        | Google Identity Services        |
| Testing               | JUnit 5, MockK, Turbine         |
| UI Testing            | Compose UI Testing              |
| Leak Detection        | LeakCanary                      |
| Performance           | Baseline Profiles               |

---

# 🧩 Habit Engine

habiTro supports multiple types of habits.

### Daily Habits

```text
Read
Workout
Meditate
Journal
```

### Weekly Habits

```text
Gym → 4 times/week
Running → 3 times/week
Reading → 5 times/week
```

### Quantified Habits

```text
Water → 3000 ml
Reading → 30 pages
Study → 120 minutes
Push-ups → 50 reps
```

---

# 🔗 Atomic Habit Chains

Multiple habits can be grouped into a sequential routine.

Example:

```text
Morning Routine

☐ Drink Water
      ↓
☐ Stretch
      ↓
☐ Read
      ↓
☐ Deep Work
```

Completing one habit advances the chain to the next step.

This allows users to treat multiple small habits as a single routine.

---

# 📊 Activity Heatmap

The analytics screen provides a visual representation of activity over time.

```text
Mon  ░ ░ ▓ ▓ █ █ ▓
Tue  ░ ▓ ▓ █ █ ▓ ░
Wed  ▓ ▓ █ █ ▓ ░ ░
Thu  ░ ▓ █ ▓ ▓ █ ▓
Fri  ▓ █ █ ▓ ░ ░ ▓
Sat  █ █ ▓ ▓ █ ▓ ░
Sun  ▓ ▓ ░ █ █ ▓ ▓
```

The actual implementation uses Jetpack Compose `Canvas` for rendering.

---

# 📜 History

Habit completion events are stored locally in Room.

Paging 3 is used for large history datasets.

```text
Room
  │
  ▼
PagingSource
  │
  ▼
Pager
  │
  ▼
Flow<PagingData<History>>
  │
  ▼
LazyPagingItems
  │
  ▼
Compose LazyColumn
```

---

# ☁️ Backup & Restore Architecture

Google Drive is used as a backup destination rather than the application's primary database.

```text
              LOCAL DEVICE
                   │
                   ▼
           ┌───────────────┐
           │ Room Database │
           └───────┬───────┘
                   │
             Backup Engine
                   │
                   ▼
          ┌─────────────────┐
          │ Backup + Metadata│
          └────────┬────────┘
                   │
                   ▼
           Google Drive
           appDataFolder
```

Example backup metadata:

```json
{
  "schemaVersion": 1,
  "appVersion": "1.0.0",
  "createdAt": "2026-01-01T12:00:00Z"
}
```

Before restoring a backup, habiTro validates the backup metadata and schema version.

---

# ⚡ Performance

habiTro is designed around local-first operations and efficient state management.

### Local-first interactions

Habit operations are performed against the local Room database instead of waiting for network requests.

### Compose state optimization

The application uses:

* `remember`
* `derivedStateOf`
* `StateFlow`
* Stable UI state models

to reduce unnecessary recompositions.

### Canvas Rendering

The activity heatmap uses Compose `Canvas` instead of creating a large number of individual layout nodes.

### Background Processing

Backup and other periodic operations are delegated to WorkManager.

---

# 🧪 Testing

Testing is organized across the application's architectural layers.

## Unit Tests

Domain logic and ViewModels can be tested independently.

Tools:

* JUnit 5
* MockK
* Turbine

Example:

```text
UseCase
   │
   ▼
Fake Repository
   │
   ▼
Expected Result
```

---

## Database Tests

Room DAOs and migrations are tested using an in-memory database.

```kotlin
Room.inMemoryDatabaseBuilder(
    context,
    HabitsDatabase::class.java
)
```

Migration tests verify that existing data remains valid when the database schema changes.

---

## Compose UI Tests

Important UI interactions can be tested using Compose testing APIs.

Examples:

* Creating a habit
* Completing a habit
* Editing a habit
* Starting a focus session
* Navigating between screens

---

## Leak Detection

[LeakCanary](https://github.com/square/leakcanary) is used during development to detect potential memory leaks.

---

# 📁 Project Structure

```text
habiTro/
│
├── gradle/
│   └── libs.versions.toml
│
├── app/
│
├── core/
│   ├── common/
│   ├── model/
│   ├── database/
│   ├── sync/
│   └── designsystem/
│
├── domain/
│   ├── repository/
│   └── usecase/
│
├── feature/
│   ├── dashboard/
│   ├── focus/
│   ├── editor/
│   ├── analytics/
│   └── backup/
│
└── README.md
```

---

# 🚀 Getting Started

## Prerequisites

Make sure you have:

* Android Studio Ladybug `2024.2.1` or newer
* JDK 17
* Android SDK 35
* Minimum Android SDK 26
* A Google Cloud project
* Google Drive API enabled

---

## Clone the Repository

```bash
git clone https://github.com/your-username/habiTro.git

cd habiTro
```

---

## Build the Project

```bash
./gradlew assembleDebug
```

---

## Run Unit Tests

```bash
./gradlew testDebugUnitTest
```

---

## Install Debug Build

```bash
./gradlew installDebug
```

---

# 🔐 Privacy

Privacy is a core design principle of habiTro.

The application is designed around:

* Local-first data storage
* Room as the primary data store
* No custom application backend for normal habit tracking
* Optional Google Drive backup
* Offline functionality for core features
* User-controlled backup data

The core habit-tracking functionality does not require an internet connection.

Internet access is required when using Google Drive backup or restore functionality.

---

# 🗺️ Roadmap

* [ ] Advanced habit statistics
* [ ] More habit scheduling rules
* [ ] Habit reminders
* [ ] Custom notification schedules
* [ ] Improved backup conflict resolution
* [ ] Backup history
* [ ] Data export/import
* [ ] CSV export
* [ ] JSON export
* [ ] Additional focus session modes
* [ ] Custom themes
* [ ] Home screen widgets
* [ ] Wear OS companion
* [ ] Advanced habit chains

---

# 🤝 Contributing

Contributions are welcome.

### 1. Fork the repository

### 2. Create a feature branch

```bash
git checkout -b feature/my-feature
```

### 3. Make your changes

### 4. Add or update tests

### 5. Commit your changes

```bash
git commit -m "feat: add my feature"
```

### 6. Push the branch

```bash
git push origin feature/my-feature
```

### 7. Open a Pull Request

---

# 📄 License

```text
Copyright (c) 2026 habiTro contributors.

Distributed under the MIT License.
```

---

# ⚡ Philosophy

> **Your habits should belong to you.**

habiTro is built around a simple idea:

```text
Local First
     ↓
Fast
     ↓
Private
     ↓
Reliable
     ↓
User Owned
```

Your habits live on your device.

You control when they are backed up.

And the application remains useful even when there is no internet connection.
