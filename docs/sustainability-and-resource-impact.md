# Sustainability & Resource Impact View

## Purpose

For each subsystem: how expensive it is to keep running (CPU/GPU load, constant background activity, and data growth
over time).
Not covered by C4 or arc42.

## K1 — Presentation.InputAdapter

- Mode: always-on (listens to input)
- Runtime cost: low CPU
- Growth: none (does not store history)

## K2 — Presentation.RenderingEngine

- Mode: active while rendering
- Runtime cost: GPU-heavy during redraws
- Growth: none (renders current board only)

## K3 — Application.InteractionController

- Mode: request/response
- Runtime cost: light to medium logic
- Growth: none (stateless coordinator)

## K4 — Domain.AnalysisService

- Mode: on-demand (move validation, evaluation, search)
- Runtime cost: high compute per request
- Growth: none (temporary calculation only)

## K5 — Core.PositionStore

- Mode: always available
- Runtime cost: low CPU
- Growth: yes (game state, move history, undo/replay data)

## Summary

K4 is the main compute hotspot.
K5 is the only subsystem with long-term growth.
K1, K2, K3 are predictable and contained.
