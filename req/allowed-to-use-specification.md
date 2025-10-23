# Allowed-to-Use Specification

## Dependency Rules

- K3 → K1 (queries input state)
- K3 → K2 (sends render commands)
- K3 → K4 (validates moves)
- K4 → K5 (reads/writes state)
- K5 depends on nothing
- K1, K2 do not depend on domain/application layers

## Data Flows

- K1 → K3: InputEvent
- K3 → K2: RenderList

## Blood Type Dependency Rules

- T (Technical) cannot depend on A or 0
- A (Application) can depend on T and 0
- 0 (Core) cannot depend on anything
