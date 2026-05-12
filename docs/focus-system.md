# Focus System Specification

## Purpose

This document defines all TV remote focus behavior.

All implementations MUST follow these rules.

Focus stability is the highest priority of this project.

---

# Core Principles

## 1. Focus must never disappear

The user must always know where focus is.

No invisible focus state is allowed.

---

## 2. Focus movement must feel predictable

Directional navigation must be consistent.

---

## 3. Focus restore is mandatory

Returning from Apps or Settings must restore previous focus position.

---

# DPAD Navigation Rules

## LEFT

Move to previous item.

If current item is first item:

* stay on current item
* do not wrap

---

## RIGHT

Move to next item.

If current item is last item:

* stay on current item
* do not wrap

---

## UP

Move to previous row.

If current row is top navigation:

* stay on current item

---

## DOWN

Move to next row.

If current row is last row:

* stay on current item

---

# BACK Behavior

## Settings Panel Open

BACK closes settings panel.

Restore previous focus.

---

## Apps Screen

BACK returns to Home screen.

Restore:

* previous row
* previous item

---

## Home Screen

BACK should not reset focus.

---

# Default Focus Rules

## Home Initial Focus

Default focus:

```text id="aq93ns"
Hero Banner Primary Action
```

OR:

```text id="b3gzw8"
First recommendation card
```

---

## Apps Screen Default Focus

Restore previous app focus if possible.

Otherwise:

* first app card

---

# Focus Restore Rules

## Must Persist

Persist inside ViewModel:

* focused row index
* focused item index

---

## Restore Timing

Restore focus ONLY after RecyclerView layout completes.

Never request focus before layout.

---

# Forbidden Behavior

## DO NOT

### 1. Use notifyDataSetChanged()

Causes focus loss.

---

### 2. Rebuild RecyclerView unnecessarily

Causes unstable focus.

---

### 3. Auto-scroll unexpectedly

User must control navigation.

---

# Focus Animation Rules

## Focus Gain

Animation:

* scale 1.0 → 1.08
* duration 150ms

---

## Focus Loss

Animation:

* scale 1.08 → 1.0
* duration 150ms

---

## Elevation

Focused item:

* slightly elevated

---

## Glow Border

Focused card:

* white subtle glow

---

# Focus State Visibility

Focused element must always be visually obvious.

Use:

* scale
* elevation
* border glow

Do NOT rely only on brightness changes.

---

# Settings Overlay Focus

When settings panel opens:

* focus moves into settings panel
* Home content loses focus

When settings panel closes:

* restore Home focus

---

# Apps Grid Focus Rules

## Horizontal Movement

Move between app cards.

---

## Vertical Movement

Move between app rows.

---

## Edge Cases

No diagonal focus jumps allowed.

---

# RecyclerView Rules

Must use:

* DiffUtil
* ListAdapter

Must avoid:

* nested focus conflicts
* excessive adapter refreshes

---

# Compose TV Rules

Compose TV may be used ONLY for:

* top navigation
* overlays
* settings panel
* animations

Main content rows MUST use Leanback.

---

# Focus Debugging Rules

During development:

Every focusable view should log:

```text id="eqd0gf"
onFocusChanged
```

to simplify debugging.

---

# Performance Rules

Focus animations must remain smooth on:

* 1GB RAM devices
* HiSilicon GPU devices

Avoid heavy GPU effects.
