# UI Animation Specification

## Purpose

This document defines all UI animation behavior.

The goal is to match Google TV interaction style.

Animations should feel:

* smooth
* calm
* premium
* responsive

NOT aggressive.

---

# Global Animation Rules

## Duration

Default:

```text id="0j7xxi"
150ms
```

Large transitions:

```text id="c19t4v"
250ms
```

---

# Interpolator

Use:

```text id="30s6zw"
FastOutSlowInInterpolator
```

for most animations.

---

# Focus Animation

## Focus Gain

Effects:

* scale up
* slight elevation
* glow border

---

## Scale

```text id="ij8wjm"
1.0 → 1.08
```

---

## Duration

```text id="q8xxfu"
150ms
```

---

# Focus Loss

## Scale

```text id="m2sdgf"
1.08 → 1.0
```

---

# Home Screen Enter Animation

## Behavior

When Home screen appears:

* slight fade in
* slight upward translation

---

## Translation

```text id="p81q1g"
translateY 20dp → 0dp
```

---

## Duration

```text id="3mfk4j"
250ms
```

---

# Recommendation Row Animation

## Horizontal Scrolling

Rows should scroll smoothly.

No sudden jumps.

---

## Card Focus

Focused card should:

* slightly enlarge
* remain centered if possible

---

# Hero Banner Animation

## Auto Transition

Hero banners may auto-rotate.

Transition:

* crossfade
* slow image transition

---

## Forbidden

Do NOT use:

* flashy zooms
* fast movement
* carousel spinning effects

---

# Settings Panel Animation

## Open

Panel slides from right.

Background slightly darkens.

---

## Close

Panel slides out smoothly.

Restore previous focus.

---

# Apps Grid Animation

## Focus

Focused app:

* scale
* elevation
* subtle glow

---

## Scrolling

Vertical scrolling must remain stable.

No bounce effect.

---

# Background Animation

## Allowed

* subtle gradients
* soft dim overlays

---

## Forbidden

* heavy blur
* realtime blur rendering
* animated particles
* excessive transparency

---

# Theme Rules

## Main Background

Use:

```text id="17xtii"
#121212
```

NOT pure black.

---

## Surface

Use:

```text id="qz9g9l"
#1E1E1E
```

---

## Text

Primary:

* white

Secondary:

* slightly dimmed

---

# Performance Rules

Animations must remain smooth on:

* HiSilicon devices
* Android 15
* low-memory TV boxes

---

# GPU Restrictions

Avoid:

* excessive shadow rendering
* realtime blur
* oversized alpha layers

---

# Transition Philosophy

Animations should feel:

* smooth
* subtle
* controlled

NOT:

* mobile-app-like
* flashy
* gaming-style

---

# Reference Material

Use:

```text id="bivw9k"
videos/reference-video.mp4
frames/
docs/storyboard.md
```

as animation reference source.
