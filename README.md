# MateMate Architecture Framework

> [!IMPORTANT]
> **This repository has been archived.** All exercises are now consolidated in the [Software-Architecture](https://github.com/ANcpLua/Software-Architecture) repository for easier navigation.
>
> **Find MateMate exercises at:**
> - Evening 3: [Service-Based Architecture (MateMate02)](https://github.com/ANcpLua/Software-Architecture/tree/main/Evening%203%20-%20Architecture%20Development/05-matemate-service-based-architecture)
> - Evening 3: [Service Elicitation Results](https://github.com/ANcpLua/Software-Architecture/tree/main/Evening%203%20-%20Architecture%20Development/matemate-service-elicitation-results)

**Extended Architecture Framework** combining C4 Model, arc42, and governance-oriented views for complete system
documentation.

**Project:** MateMate - Desktop Chess Application
**Framework Version:** 1.0
**Date:** November 13, 2025

---

## Framework Overview

This architectural documentation demonstrates an **extended framework** that surpasses both C4 Model and arc42
individually by combining their strengths and adding critical governance dimensions.

### Framework Composition

**From C4 Model (2 of 4 levels):**

- ✅ **C1: System Context** - System boundary and external actors
- ✅ **C2: Container View** - Deployable units and technology choices
- ❌ C3: Component - Redundant with subsystem decomposition
- ❌ C4: Code - Too low-level, maintained by IDE

**From arc42 (7 of 12 chapters):**

- ✅ **Chapter 1: Introduction & Goals** - Purpose, quality goals, stakeholders
- ✅ **Chapter 3: Context & Scope** - Business and technical context
- ✅ **Chapter 5: Building Blocks** - Component decomposition
- ✅ **Chapter 6: Runtime View** - Dynamic behavior and use cases
- ✅ **Chapter 8: Cross-Cutting Concepts** - Architectural patterns
- ✅ **Chapter 9: Design Decisions** - Architecture Decision Records (ADRs)
- ✅ **Chapter 10: Quality Requirements** - Quality tree and scenarios

**Governance Extensions (6 additions beyond C4 and arc42):**

1. **Allowed-to-Use Matrix** - Binary dependency permission matrix with automated checking
2. **Change Impact Heatmap** - Scenario-based blast radius analysis
3. **Sustainability View** - Runtime resource footprint per subsystem
4. **FinOps View** - Cost drivers and scaling behavior
5. **Metamodel** - Explicit architectural concepts and relationships
6. **Blood Type Architecture** - T/A/0 classification for change management

### Visual Semantic System

**All diagram elements use meaningful visual encoding:**

**Color Semantics:**

- 🟦 **Blue (TYPE T)** - Technical subsystems (change driver: technology evolution)
- 🟪 **Purple (TYPE A)** - Application subsystems (change driver: business rules)
- 🟧 **Orange (TYPE 0)** - Core subsystems (universal concepts, rarely change)
- **Note:** All colored boxes use black text for optimal readability

**Frame Style Semantics:**

- **Solid border** - Stable subsystem (< 5 changes/year)
- **Dashed border** - Evolving subsystem (5-20 changes/year)
- **Dotted border** - Volatile subsystem (> 20 changes/year)
- **Thick border (4px)** - Security/architectural boundary

**Size Semantics:**

- **Width** - Lines of Code (LOC)
- **Height** - Number of dependencies (fan-in + fan-out)

**Arrow Semantics:**

- **Solid arrow** - Compile-time dependency (direct imports/calls)
- **Dashed arrow** - Runtime dependency (events, messages)
- **Green arrow** - Allowed dependency (passes Allowed-to-Use Matrix)
- **Red arrow** - Forbidden dependency (violation)
- **Thickness** - Coupling strength (number of calls)

---

## Architecture Review Process

This extended framework enables more precise and actionable architecture reviews than C4 or arc42 alone:

### Review Workflow

**1. Context & Constraints Review**

- Verify system boundary (C4 System Context)
- Validate external interfaces
- Check technology constraints

**2. Building Blocks & Boundaries Review**

- Verify subsystem decomposition (arc42 Building Blocks)
- Check blood type classification (T/A/0)
- Validate separation of concerns

**3. Dependency Correctness Review**

- Check Allowed-to-Use Matrix compliance
- Detect forbidden dependencies (automated)
- Measure coupling metrics

**4. Runtime Behavior Review**

- Analyze sequence diagrams (arc42 Runtime View)
- Verify error handling paths
- Check performance characteristics

**5. Change Resilience Review**

- Analyze Change Impact Heatmap
- Estimate blast radius for scenarios
- Identify architectural fragility

**6. Operational Footprint Review**

- Check Sustainability View metrics
- Validate resource consumption
- Assess carbon footprint

**7. Cost Scalability Review**

- Analyze FinOps View
- Identify cost drivers
- Validate scaling assumptions

**8. Visual Semantic Review**

- Verify color/frame/size consistency
- Detect boundary violations visually
- Check diagram comprehensibility

### Review Output

Each review produces:

- ✅ Pass/Fail status per subsystem
- 📊 Quantified metrics (not subjective assessments)
- 🔴 Violations list with remediation steps
- 📈 Trend analysis (improving/stable/degrading)

---

## Quick Navigation

### Requirements & Specifications

- **[Subsystems](req/subsystems.md)** - K1-K5 with blood types, responsibilities, and services
- **[Services](req/services.md)** - 20 services mapped to subsystems
- **[Allowed-to-Use Specification](req/allowed-to-use-specification.md)** - Dependency rules and data flows

### C4 Architecture Diagrams

- **[C1: System Context](c4/c1-system-context.md)** - MateMate system boundary
- **[C2: Container View](c4/c2-container-view.md)** - K1-K5 subsystems with dependencies

### arc42 Documentation

- **[Chapter 1: Introduction](arc42/01-introduction.md)** - Purpose, quality goals, stakeholders
- **[Chapter 3: Context & Scope](arc42/03-context-scope.md)** - Business and technical context
- **[Chapter 5: Building Blocks](arc42/05-building-blocks.md)** - Subsystem decomposition
- **[Chapter 6: Runtime View](arc42/06-runtime-view.md)** - Use case sequences and state machines
- **[Chapter 8: Cross-Cutting](arc42/08-cross-cutting.md)** - Architectural patterns
- **[Chapter 9: Design Decisions](arc42/09-design-decisions.md)** - ADRs with rationale
- **[Chapter 10: Quality Requirements](arc42/10-quality-requirements.md)** - Quality tree and scenarios

### Governance Documentation

- **[Metamodel](docs/metamodel.md)** - Architectural concepts and relationships
- **[Allowed-to-Use Matrix](docs/allowed-to-use-matrix.md)** - Dependency permission matrix
- **[Change Impact Heatmap](docs/change-impact-heatmap.md)** - Scenario-based blast radius
- **[Sustainability & Resource Impact](docs/sustainability-and-resource-impact.md)** - Runtime footprint
- **[FinOps & Cost Governance](docs/finops-and-cost-governance.md)** - Cost drivers
- **[New Concepts](docs/new-concepts.md)** - Summary of framework extensions

---

## MateMate System Overview

### Purpose

Desktop chess application with full FIDE rule compliance and graphical user interface.

### Subsystems (5)

| ID     | Name                  | Blood Type | Role                                     | Services | LOC    |
|--------|-----------------------|------------|------------------------------------------|----------|--------|
| **K1** | InputAdapter          | T          | Captures mouse/keyboard events           | 2        | ~500   |
| **K2** | RenderingEngine       | T          | Renders board and pieces to screen       | 4        | ~1,200 |
| **K3** | InteractionController | A          | Orchestrates game flow and UI logic      | 2        | ~800   |
| **K4** | AnalysisService       | A          | Chess rules, move validation, evaluation | 6        | ~2,500 |
| **K5** | PositionStore         | 0          | Immutable game state and history         | 6        | ~600   |

**Total:** 5 subsystems, 20 services, ~5,600 LOC

### Key Architectural Decisions

**Blood Type Separation:**

- TYPE T subsystems (K1, K2) change when technology evolves
- TYPE A subsystems (K3, K4) change when business rules evolve
- TYPE 0 subsystems (K5) rarely change (universal concepts)

**Dependency Direction:**

- K3 orchestrates K1, K2, K4 (application layer)
- K4 depends only on K5 (rules engine isolated from UI)
- K5 has zero dependencies (pure core)

**Allowed Dependencies:**

- K3 → K1, K2, K4 ✅
- K4 → K5 ✅
- All others forbidden ❌

---

## Framework Benefits

### Compared to C4 Alone

- ✅ Adds dependency enforcement (Allowed-to-Use Matrix)
- ✅ Adds change impact analysis (Heatmap)
- ✅ Adds cost/resource tracking (FinOps, Sustainability)
- ✅ Adds quality scenarios (not just structure)

### Compared to arc42 Alone

- ✅ Adds visual semantics (color, frame, size have meaning)
- ✅ Adds automated checks (dependency violations)
- ✅ Adds governance views (cost, sustainability)
- ✅ More concise (7 chapters vs 12, zero redundancy)

### Compared to Both

- ✅ Combines structural clarity (C4) with completeness (arc42)
- ✅ Adds governance dimensions neither framework has
- ✅ Enables automated architecture reviews
- ✅ Optimizes for evolution, not just documentation

---

## How to Read This Documentation

### For Stakeholders (15 minutes)

1. Read this README
2. View [C1: System Context](c4/c1-system-context.md)
3. Review [Quality Requirements](arc42/10-quality-requirements.md)

### For Architects (1 hour)

1. All of the above
2. Study [C2: Container View](c4/c2-container-view.md)
3. Read [Building Blocks](arc42/05-building-blocks.md)
4. Review [Design Decisions](arc42/09-design-decisions.md)
5. Check [Allowed-to-Use Matrix](docs/allowed-to-use-matrix.md)

### For Developers (2 hours)

1. All of the above
2. Study [Runtime View](arc42/06-runtime-view.md)
3. Read [Cross-Cutting Concepts](arc42/08-cross-cutting.md)
4. Review [Metamodel](docs/metamodel.md)
5. Check [Services Mapping](req/services.md)

### For Reviewers (30 minutes)

1. Check [Allowed-to-Use Matrix](docs/allowed-to-use-matrix.md) - Any violations?
2. Review [Change Impact Heatmap](docs/change-impact-heatmap.md) - Blast radius acceptable?
3. Check [Quality Requirements](arc42/10-quality-requirements.md) - Scenarios verified?
4. Review [Sustainability View](docs/sustainability-and-resource-impact.md) - Resource usage reasonable?

---

## Metrics Summary

**Note:** Metric values represent design targets and analytical estimates based on architectural analysis, not runtime
measurements from automated tooling. Values are manually maintained and updated with each architectural change.

### Architecture Health

| Metric                             | Target   | Actual | Status |
|------------------------------------|----------|--------|--------|
| **Dependency Violations**          | 0        | 0      | ✅ Pass |
| **Subsystem Count**                | 3-7      | 5      | ✅ Pass |
| **Avg Dependencies per Subsystem** | < 5      | 3.6    | ✅ Pass |
| **Total LOC**                      | < 10,000 | ~5,600 | ✅ Pass |
| **Blood Type Separation**          | 100%     | 100%   | ✅ Pass |

### Change Impact

| Scenario            | Affected Subsystems | Effort | Risk   |
|---------------------|---------------------|--------|--------|
| Renderer swap       | K2, K3              | 80h    | High   |
| Chess rule change   | K4, K5              | 16h    | Medium |
| Input device change | K1, K3              | 24h    | Medium |

### Resource Consumption

| Subsystem | CPU                | Memory | Storage Growth    | Cost/Month* |
|-----------|--------------------|--------|-------------------|-------------|
| K1        | 1-2%               | 50 MB  | None              | $2          |
| K2        | 5-15% (GPU 20-40%) | 200 MB | None              | $15         |
| K3        | 2-5%               | 100 MB | None              | $5          |
| K4        | **40-80%**         | 500 MB | None              | **$80**     |
| K5        | 1-2%               | 100 MB | +50 MB/1000 games | **$50**     |

*For the FinOps governance extension, we model a hypothetical cloud/SaaS deployment scenario with 10,000 concurrent
users to illustrate cost behavior and scaling characteristics. The current implementation is a local desktop
application.

---

## Framework Comparison

| Feature                    | C4       | arc42 | **This Framework**        |
|----------------------------|----------|-------|---------------------------|
| System Context             | ✅        | ✅     | ✅ C1                      |
| Container View             | ✅        | ❌     | ✅ C2                      |
| Runtime View               | ❌        | ✅     | ✅ Ch6                     |
| Quality Scenarios          | ❌        | ✅     | ✅ Ch10                    |
| ADRs                       | ❌        | ✅     | ✅ Ch9                     |
| **Dependency Enforcement** | ❌        | ❌     | ✅ Allowed-to-Use Matrix   |
| **Change Impact**          | ❌        | ❌     | ✅ Heatmap                 |
| **Cost/Resource Tracking** | ❌        | ❌     | ✅ FinOps + Sustainability |
| **Visual Semantics**       | ⚠️ Basic | ❌     | ✅ Color + Frame + Size    |
| **Automated Checks**       | ❌        | ❌     | ✅ Dependency violations   |

---

## Document Generation

This documentation can be automatically converted to PDF:

```bash
# Uses GitHub Actions workflow (.github/workflows/mdtopdf.yml)
git push
# PDF generated in /images/ folder
```
