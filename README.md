# MateMate Architecture Framework

Organized documentation following C4 + arc42 principles with additional concepts.

## Quick Start

1. **Requirements** → `req/` - Start here for subsystems, services, dependencies
2. **C4 Diagrams** → `c4/` - Visual architecture (system context, containers)
3. **arc42 Chapters** → `arc42/` - Detailed documentation
4. **Framework Docs** → `docs/` - New concepts, visual semantics, review guide

## Directory Structure

```
.
├── req/                      # Requirements
│   ├── subsystems.md         # K1-K5 with blood types
│   ├── services.md           # 20 services mapped
│   └── allowed-to-use.md     # Dependency rules
│
├── c4/                       # C4 Architecture
│   ├── c1-system-context.md  # Level 1
│   ├── c2-container-view.md  # Level 2
│   └── images/               # Rendered diagrams
│       ├── c1.png
│       └── c2.png
│
├── arc42/                    # arc42 Documentation
│   ├── 01-introduction.md    # Chapter 1
│   ├── 03-context-scope.md   # Chapter 3
│   ├── 05-building-blocks.md # Chapter 5
│   └── 08-cross-cutting.md   # Chapter 8
│
└── docs/                        # Framework Documentation
    ├── new-concepts.md          # 2 innovative concepts
    ├── allowed-to-use-matrix.md # Binary dependency matrix
    ├── change-impact-heatmap.md # Scenario-based impact analysis
    ├── visual-semantics.md      # Diagram visual definitions
    └── review-usage.md          # 4-step review process
```

## Framework Overview

### Requirements (req/)
- **[subsystems.md](req/subsystems.md)** - 5 subsystems (K1-K5) with blood types T/A/0, Einsatzbeschreibung, knowledge
- **[services.md](req/services.md)** - 20 services mapped to subsystems
- **[allowed-to-use.md](req/allowed-to-use.md)** - Dependency rules, data flows, blood type constraints

### C4 Architecture (c4/)
- **[c1-system-context.md](c4/c1-system-context.md)** - System boundary, external actors
- **[c2-container-view.md](c4/c2-container-view.md)** - 5 containers with dependencies
- **[images/](c4/images/)** - Rendered diagrams ([c1.png](c4/images/c1.png), [c2.png](c4/images/c2.png))

### arc42 Documentation (arc42/)
- **[01-introduction.md](arc42/01-introduction.md)** - Purpose, quality goals, stakeholders
- **[03-context-scope.md](arc42/03-context-scope.md)** - Business and technical context
- **[05-building-blocks.md](arc42/05-building-blocks.md)** - Building block view with all subsystems
- **[08-cross-cutting.md](arc42/08-cross-cutting.md)** - Cross-cutting concepts

### Framework Documentation (docs/)
- **[new-concepts.md](docs/new-concepts.md)** - 2 innovative additions:
  1. Allowed-to-Use Matrix (binary dependency permission contract)
  2. Change Impact Heatmap (scenario-based blast radius prediction)
- **[allowed-to-use-matrix.md](docs/allowed-to-use-matrix.md)** - 5×5 binary permission matrix
- **[change-impact-heatmap.md](docs/change-impact-heatmap.md)** - 5 pre-analyzed change scenarios
- **[visual-semantics.md](docs/visual-semantics.md)** - Color, line, shape definitions for diagrams
- **[review-usage.md](docs/review-usage.md)** - 4-step architecture review process
