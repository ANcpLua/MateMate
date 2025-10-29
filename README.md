# MateMate Architecture Framework

Organized documentation following C4 + arc42 principles with MateMate-specific extensions.

## Quick Start

1. **Requirements** → `req/`
   Subsystems, services, and allowed-to-use rules.

2. **C4 Diagrams** → `c4/`
   System context and container view.

3. **arc42 Chapters** → `arc42/`
   Stakeholders, scope, building blocks, cross-cutting concerns.

4. **Framework Docs** → `docs/`
   Additional concepts (dependency contracts, impact analysis, sustainability, cost).

## Directory Structure

```
.
├── req/                           # Requirements
│   ├── subsystems.md              # K1–K5 with blood types
│   ├── services.md                # 20 services mapped
│   └── allowed-to-use-specification.md
│                                   # Dependency rules and data flows
│
├── c4/                            # C4 Architecture
│   ├── c1-system-context.md       # Level 1 (System Context / C1) with PlantUML
│   ├── c2-container-view.md       # Level 2 (Container View / C2) with PlantUML
│   └── images/                    # Generated diagrams (not in git)
│
├── arc42/                         # arc42 Documentation
│   ├── 01-introduction.md         # Chapter 1
│   ├── 03-context-scope.md        # Chapter 3
│   ├── 05-building-blocks.md      # Chapter 5
│   └── 08-cross-cutting.md        # Chapter 8
│
└── docs/                          # Framework Documentation
    ├── new-concepts.md                    # Four architecture extensions
    ├── allowed-to-use-matrix.md           # Binary dependency permission matrix
    ├── change-impact-heatmap.md           # Scenario-based impact analysis
    ├── sustainability-and-resource-impact.md
    │                                      # Runtime footprint per subsystem
    └── finops-and-cost-governance.md      # Cost pressure per subsystem
```

## Requirements (req/)
- **[subsystems.md](req/subsystems.md)**
  Five subsystems (K1–K5) with blood types T / A / 0, role, and knowledge.
- **[services.md](req/services.md)**
  20 services mapped to subsystems.
- **[allowed-to-use-specification.md](req/allowed-to-use-specification.md)**
  Dependency rules, allowed flows, and blood type constraints.

## C4 Architecture (c4/)
- **[c1-system-context.md](c4/c1-system-context.md)**
  System boundary and external actor. Contains PlantUML source code.
- **[c2-container-view.md](c4/c2-container-view.md)**
  Internal containers and data flows. Contains PlantUML source code.
- **images/** - Diagrams auto-generated from PlantUML during PDF build (not in git).

## arc42 Documentation (arc42/)
- **[01-introduction.md](arc42/01-introduction.md)**
  Purpose, quality goals, stakeholders.
- **[03-context-scope.md](arc42/03-context-scope.md)**
  Business and technical context.
- **[05-building-blocks.md](arc42/05-building-blocks.md)**
  Building block view with all subsystems.
- **[08-cross-cutting.md](arc42/08-cross-cutting.md)**
  Cross-cutting concepts.

## Framework Documentation (docs/)
- **[new-concepts.md](docs/new-concepts.md)**
  Summary of MateMate-specific extensions beyond C4/arc42:
  - Allowed-to-Use Matrix (dependency contract)
  - Change Impact Heatmap (blast radius by scenario)
  - Sustainability & Resource Impact View (runtime footprint)
  - FinOps & Cost Governance View (cost drivers)
- **[allowed-to-use-matrix.md](docs/allowed-to-use-matrix.md)**
  Binary permission matrix, one cell per subsystem pair.
- **[change-impact-heatmap.md](docs/change-impact-heatmap.md)**
  Subsystem impact per common change scenario.
- **[sustainability-and-resource-impact.md](docs/sustainability-and-resource-impact.md)**
  Runtime cost profile (always-on, compute intensity, data growth).
- **[finops-and-cost-governance.md](docs/finops-and-cost-governance.md)**
  Cost sensitivity per subsystem.
