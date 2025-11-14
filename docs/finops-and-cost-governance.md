# FinOps & Cost Governance View

## Purpose

For each subsystem: what makes it expensive if it is hosted at scale (CPU/GPU time, storage growth, bandwidth).
This is cost awareness at architecture level. Traditional C4/arc42 do not assign cost drivers to specific elements.

## K1 — Presentation.InputAdapter

- Cost driver: basic event handling
- Scaling: basically fixed; cheap even with many users

## K2 — Presentation.RenderingEngine

- Cost driver: rendering work (GPU / frame rate / resolution)
- Scaling: increases with visual fidelity and concurrent sessions

## K3 — Application.InteractionController

- Cost driver: orchestration logic
- Scaling: light per interaction; effectively negligible

## K4 — Domain.AnalysisService

- Cost driver: per-move calculation and search depth
- Scaling: increases with number of analysis requests
- Note: main variable cost in the system

## K5 — Core.PositionStore

- Cost driver: retained game state and history
- Scaling: increases with how much we store and how long we keep it

## Summary

If anything becomes expensive at scale, it is:

- K4 (analysis per move)
- K5 (stored history over time)
  The others are operationally cheap.
