# Google TV Style Launcher - Claude Code Development Guide

## Project Overview

This project is an Android TV Launcher inspired by Google TV.

Target platform:

* HiSilicon SoC
* Android 15 AOSP
* Remote control based navigation
* Google Services integrated
* Google Assistant launched via Intent
* Overseas TV Box market

This is NOT a full Google TV OS implementation.

The goal is:

1. Google TV style UI
2. Stable DPAD focus system
3. Recommendation rows
4. Apps page
5. Google Assistant launch
6. Settings overlay
7. Smooth TV animations

NOT included:

* Hotword ("Hey Google")
* System-level Assistant
* HDMI Input
* Operator Tier integration
* Google TV certification features

---

# Repository Structure

Repository:

https://github.com/amosliuster163/google-tv-launcher

Important reference files:

```text
README.md
docs/PRD.md
docs/storyboard.md
frames/
videos/reference-video.mp4
```

These files MUST be treated as the source of truth for UI behavior.

---

# Development Principles

## MUST FOLLOW

### 1. TV Focus Stability First

TV UI is focus-driven.

Focus stability is more important than visuals.

All implementations must prioritize:

* DPAD navigation
* Focus restore
* Stable RecyclerView behavior
* No focus loss during updates

---

### 2. Leanback Handles Core TV Rows

Do NOT implement all rows using Compose TV.

Use:

* Leanback for content rows
* Compose for top navigation
* Compose for overlays
* Compose for animations

---

### 3. Never Use notifyDataSetChanged()

Forbidden:

```kotlin
adapter.notifyDataSetChanged()
```

Must use:

* DiffUtil
* ListAdapter
* AsyncListDiffer

---

### 4. Single Activity Architecture

Use:

```text
MainActivity
 + Fragments
```

Do NOT create multiple Activities for Launcher pages.

---

### 5. Feature-first Architecture

Use:

```text
feature/home
feature/apps
feature/settings
feature/assistant
```

Do NOT create huge shared UI folders.

---

# Technical Stack

## Language

Kotlin

---

## Architecture

MVVM

---

## UI Stack

Hybrid:

* Leanback
* Jetpack Compose
* Compose TV

---

## Navigation

Navigation Component

---

## Image Loading

Coil

---

## Local Storage

DataStore

---

## Async

Kotlin Coroutines

---

# Recommended Module Structure

```text
app/
│
├── core/
│   ├── ui/
│   ├── theme/
│   ├── focus/
│   └── utils/
│
├── data/
│   ├── apps/
│   ├── recommendations/
│   └── settings/
│
├── feature/
│   ├── home/
│   ├── apps/
│   ├── settings/
│   └── assistant/
│
└── MainActivity.kt
```

---

# UI Reference Rules

## IMPORTANT

The following references MUST be used:

```text
docs/storyboard.md
frames/
videos/reference-video.mp4
```

All UI implementations must visually match these references.

---

# Frame Naming Rules

All frame references should use semantic names.

BAD:

```text
001.png
002.png
```

GOOD:

```text
home_idle.png
home_recommendation.png
apps_grid.png
settings_panel.png
```

---

# Storyboard Interpretation Rules

Each storyboard frame represents:

* Screen layout
* Focus state
* Animation state
* Navigation state
* Overlay state

Implementations must follow storyboard behavior exactly.

---

# Home Screen Specification

## Reference

```text
docs/storyboard.md
frames/home_idle.png
frames/home_recommendation.png
```

---

## Layout Structure

```text
Top Navigation
Hero Banner
Recommendation Rows
Favorite Apps Row
```

---

## Implementation Rules

### Top Navigation

Use Compose.

Tabs:

* For You
* Movies
* Shows
* Apps

---

### Recommendation Rows

Use:

```kotlin
RowsSupportFragment
```

NOT custom nested RecyclerViews.

---

### Favorite Apps Row

Use:

* HorizontalGridView
  OR
* Leanback row

---

## Focus Animation

Focused item:

* scale 1.0 → 1.08
* duration 150ms
* slight elevation increase

---

## Focus Restore

Home screen must restore:

* previous row
* previous card

when returning from apps.

---

# Apps Screen Specification

## Reference

```text
frames/apps_grid.png
```

---

## Requirements

Display installed TV apps.

Use:

```kotlin
Intent.CATEGORY_LEANBACK_LAUNCHER
```

---

## Layout

Use:

```kotlin
TvLazyVerticalGrid
```

OR

```kotlin
VerticalGridSupportFragment
```

---

## App Card Behavior

Focused app:

* scale up
* glow border
* elevated shadow

---

# Settings Panel Specification

## Reference

```text
frames/settings_panel.png
```

---

## Behavior

Right-side overlay panel.

Animation:

```text
slide from right
fade in background
```

---

## Implementation

Use Compose:

```kotlin
AnimatedVisibility
```

---

# Focus System Rules

## VERY IMPORTANT

TV UX quality depends on focus stability.

---

# Navigation Rules

LEFT:
Previous item

RIGHT:
Next item

UP:
Previous row

DOWN:
Next row

BACK:
Restore previous focus

---

# Focus Persistence

Must persist:

* focused row index
* focused item index

inside ViewModel.

---

# Focus Animation Rules

Duration:

```text
150ms
```

Scale:

```text
1.0 -> 1.08
```

Interpolator:

```text
FastOutSlowInInterpolator
```

---

# Google Assistant Integration

## Goal

Support remote control voice button launch.

NOT full Assistant integration.

---

# Remote Key

Use:

```kotlin
KeyEvent.KEYCODE_VOICE_ASSIST
```

---

# Launch Method

Use:

```kotlin
Intent(Intent.ACTION_VOICE_COMMAND)
```

Fallback:

```kotlin
Intent("android.intent.action.ASSIST")
```

---

# Requirements

Google App must be installed.

Google Play Services must exist.

---

# Installed Apps Retrieval

Use:

```kotlin
Intent(Intent.ACTION_MAIN).apply {
    addCategory(Intent.CATEGORY_LEANBACK_LAUNCHER)
}
```

---

# Recommendation Data

## Phase 1

Use local JSON.

```text
assets/recommendations.json
```

---

## Future

Can migrate to:

* Firebase
* REST API
* CMS

---

# Animation Rules

## Allowed

* scale
* alpha
* translation

---

## Forbidden

* realtime blur
* heavy shadow rendering
* complex MotionLayout scenes
* excessive GPU effects

HiSilicon GPU performance is limited.

---

# Performance Rules

## RecyclerView

Must use:

```kotlin
setHasFixedSize(true)
```

Avoid deep nested RecyclerViews.

---

## Images

Must use Coil caching.

---

## Rows

Do NOT preload excessive rows.

---

# Forbidden Implementations

## DO NOT

### 1. Pure Compose TV Launcher

Focus is not stable enough.

---

### 2. notifyDataSetChanged()

Causes focus loss.

---

### 3. Deep RecyclerView nesting

Causes frame drops.

---

### 4. Multiple Activities

Causes focus reset and black flashes.

---

# MVP Scope

## Included

* Home
* Apps
* Settings Overlay
* Google Assistant launch
* Recommendation rows
* Focus system

---

## Excluded

* HDMI Input
* Live Channels
* Full Search Provider
* Watch Next integration
* Google Home control
* Hotword
* Operator Tier

---

# Development Order

## Phase 1

### Step 1

Create:

```text
MainActivity
Navigation
Theme
```

---

### Step 2

Implement:

```text
HomeFragment
Rows
Focus system
```

---

### Step 3

Implement:

```text
AppsFragment
Installed apps
```

---

### Step 4

Implement:

```text
Settings overlay
```

---

### Step 5

Implement:

```text
Assistant launch
```

---

# Build Gradle Dependencies

```kotlin
dependencies {

    implementation("androidx.leanback:leanback:1.2.0")

    implementation("androidx.tv:tv-foundation:1.0.0")

    implementation("androidx.tv:tv-material:1.0.0")

    implementation("androidx.compose.ui:ui:1.7.0")

    implementation("androidx.navigation:navigation-fragment-ktx:2.8.0")

    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.8.4")

    implementation("androidx.datastore:datastore-preferences:1.1.1")

    implementation("io.coil-kt:coil-compose:2.7.0")
}
```

---

# Code Quality Rules

All generated code must include:

* imports
* comments
* lifecycle handling
* focus handling
* null safety
* ViewModel separation

---

# IMPORTANT

Every implementation task must reference:

* storyboard frame
* corresponding screenshot
* expected focus behavior
* expected animation behavior

Never generate UI without reference matching.
