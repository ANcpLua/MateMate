# Innovative Additions (Not in C4 or arc42)

Four concepts added to standard C4/arc42 that we treat as first-class:

## 1. Allowed-to-Use Matrix

Defines which subsystem may depend on which other subsystem.
This is a permission model, not just documentation.
C4 can show "what calls what", but it does not express "what is allowed".
See [allowed-to-use-matrix.md](allowed-to-use-matrix.md)

## 2. Change Impact Heatmap

Shows expected blast radius of typical change scenarios across subsystems.
Used for planning and risk discussion before doing work.
C4 and arc42 do not provide a per-scenario impact table.
See [change-impact-heatmap.md](change-impact-heatmap.md)

## 3. Sustainability & Resource Impact View

Shows runtime footprint per subsystem:

- always-on vs. on-demand
- CPU/GPU intensity
- data that keeps growing
  C4 and arc42 do not label components by runtime cost to run.
  See [sustainability-and-resource-impact.md](sustainability-and-resource-impact.md)

## 4. FinOps & Cost Governance View

Shows cost pressure per subsystem:

- main cost driver (CPU, GPU, storage, etc.)
- does cost scale per user / per move, or is it basically fixed
  C4 and arc42 do not attach cost drivers to architecture elements.
  See [finops-and-cost-governance.md](finops-and-cost-governance.md)
