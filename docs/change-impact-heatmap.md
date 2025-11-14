# Change Impact Heatmap

Matrix showing which subsystems are affected by common change scenarios.

| Scenario                                         | K1 | K2 | K3 | K4 | K5 | Notes                                      |
|--------------------------------------------------|----|----|----|----|----|--------------------------------------------|
| **Renderer swap** (e.g., OpenGL → Vulkan)        | ✗  | ✓✓ | ✓  | ✗  | ✗  | K2 reworked; K3 adapts to new API          |
| **Chess rule change** (e.g., variant castling)   | ✗  | ✗  | ✗  | ✓✓ | ✓  | K4 logic + K5 state flags updated          |
| **State format change** (e.g., add zobrist hash) | ✗  | ✗  | ✗  | ✓  | ✓✓ | K5 schema change; K4 queries update        |
| **Input device change** (e.g., add touch)        | ✓✓ | ✗  | ✓  | ✗  | ✗  | K1 captures new events; K3 maps to squares |
| **Add new piece type**                           | ✗  | ✓  | ✓  | ✓✓ | ✓  | K4 rules, K5 state, K3 mapping, K2 sprite  |

**Legend**: ✓✓ = high impact; ✓ = medium; ✗ = none

## Usage

Before making architectural changes:

1. Identify the change scenario
2. Check the heatmap for affected subsystems
3. Plan changes in order: 0 → A → T (reverse dependency flow)
4. Verify no unexpected subsystems are affected
