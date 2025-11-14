# C4 Level 2: Container View

![C2 Container View](images/c2.png)

## Containers

| ID | Name                  | Blood Type | Role                       |
|----|-----------------------|------------|----------------------------|
| K1 | InputAdapter          | T          | Captures input events      |
| K2 | RenderingEngine       | T          | Renders board              |
| K3 | InteractionController | A          | Orchestrates game flow     |
| K4 | AnalysisService       | A          | Chess rules and validation |
| K5 | PositionStore         | 0          | Game state storage         |

## Dependencies

- K3 → K1, K2, K4 (K3 orchestrates all three)
- K4 → K5 (rules engine reads/writes state)
- K1 → K3 (events: input)
- K3 → K2 (events: render)

## Legend

- 🔵 **T (Technical)** - Blue: Cannot depend on A or 0
- 🟣 **A (Application)** - Purple: Can depend on T and 0
- 🟠 **0 (Core)** - Orange: Cannot depend on anything
- **Solid lines** = Dependencies (direct imports/calls)
- **Dashed lines** = Events (pub/sub, no direct coupling)
