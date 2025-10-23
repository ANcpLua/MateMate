# MateMate Subsystems

## K1 Presentation.InputAdapter

**Blood Type**: T (Technical)

**Einsatzbeschreibung**: Captures mouse/keyboard; exposes last click/drop.

**Knowledge**:
- OS input APIs
- pixel coords
- screen bounds
- No chess

**Services**: 1, 16

---

## K2 Presentation.RenderingEngine

**Blood Type**: T (Technical)

**Einsatzbeschreibung**: Renders primitives and positions from render lists.

**Knowledge**:
- sprites/bitmaps
- batching
- scaling
- frame timing
- No chess

**Services**: 5, 6, 12, 18

---

## K3 Application.InteractionController

**Blood Type**: A (Application)

**Einsatzbeschreibung**: Interprets UI input as board intents; prepares render lists from domain snapshots via the Analysis API.

**Knowledge**:
- board layout
- pixel↔square
- piece↔sprite
- orientation/themes
- No persistence

**Services**: 3, 13

---

## K4 Domain.AnalysisService

**Blood Type**: A (Application)

**Einsatzbeschreibung**: Unified chess logic API: move legality, game state validation, and position evaluation/best move.

**Knowledge**:
- FIDE rules (movement, checks, castling)
- evaluation heuristics
- search

**Services**: 2, 4, 7, 8, 10, 11

---

## K5 Core.PositionStore

**Blood Type**: 0 (Core)

**Einsatzbeschreibung**: Canonical game state and history; immutable queries; no "may I?" logic.

**Knowledge**:
- board representation
- side-to-move
- castling flags
- move log/history

**Services**: 9, 14, 15, 17, 19, 20
