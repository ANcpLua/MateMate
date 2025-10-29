# C4 Level 2: Container View

![C2 Container View](images/c2.png)

## Containers

| ID | Name | Blood Type | Role |
|----|------|------------|------|
| K1 | InputAdapter | T | Captures input events |
| K2 | RenderingEngine | T | Renders board |
| K3 | InteractionController | A | Orchestrates game flow |
| K4 | AnalysisService | A | Chess rules and validation |
| K5 | PositionStore | 0 | Game state storage |

## Dependencies

- K3 → K1, K2, K4 (K3 orchestrates all three)
- K4 → K5 (rules engine reads/writes state)
- K1 → K3 (events: input)
- K3 → K2 (events: render)

## Diagram

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title MateMate Container View (C2)

AddElementTag("technical", $bgColor="#B3D9FF", $borderColor="#1E5AA8", $fontColor="#000000", $legendText="T (Technical) - Cannot depend on A or 0")
AddElementTag("application", $bgColor="#D9B3FF", $borderColor="#6A3FB2", $fontColor="#000000", $legendText="A (Application) - Can depend on T and 0")
AddElementTag("core", $bgColor="#FFD699", $borderColor="#CC8800", $fontColor="#000000", $legendText="0 (Core) - Cannot depend on anything")

AddRelTag("dependency", $lineColor="#555555", $lineStyle="SolidLine", $legendText="Dependency (direct imports/calls)")
AddRelTag("event", $lineColor="#0066CC", $lineStyle="DashedLine", $legendText="Event (pub/sub, no direct coupling)")

HIDE_STEREOTYPE()

Person(player, "Chess Player", "Human user making moves")

System_Boundary(matemate, "MateMate") {
    Container(k1, "K1: InputAdapter", "OS Events", "Polls keyboard/mouse, translates to domain events", $tags="technical")
    Container(k2, "K2: RenderingEngine", "2D Graphics", "Transforms Position to pixels, manages sprites/fonts", $tags="technical")
    Container(k3, "K3: InteractionController", "Game Loop", "Main loop: reads input, updates state, triggers render", $tags="application")
    Container(k4, "K4: AnalysisService", "Chess Engine", "Move validation, check/mate detection, legal moves", $tags="application")
    Container(k5, "K5: PositionStore", "FEN + History", "Current board state, move history, game metadata", $tags="core")
}

Rel(player, k1, "Clicks/drags pieces", "Mouse/keyboard")
Rel(k2, player, "Displays board state", "Screen")

Rel_D(k1, k3, "InputEvent", $tags="event")
Rel_D(k3, k2, "RenderCommand", $tags="event")

Rel_U(k3, k1, "isPressed(key)", $tags="dependency")
Rel_D(k3, k2, "render(position)", $tags="dependency")
Rel_R(k3, k4, "getLegalMoves()", $tags="dependency")
Rel_D(k4, k5, "getPosition() / saveMove()", $tags="dependency")

SHOW_LEGEND()

@enduml
```
