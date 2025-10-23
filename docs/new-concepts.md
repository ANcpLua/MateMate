# Innovative Additions (Not in C4 or arc42)

Two concepts added to standard C4/arc42 that are genuinely new:

## 1. Allowed-to-Use Matrix

**What it is**: Binary dependency permission matrix (5×5) showing which subsystems may depend on others.

**Not in C4/arc42**:
- C4 shows dependencies but has no permission concept
- arc42 documents dependencies but doesn't enforce via matrix

**Innovation**: Explicit ✓/✗ matrix makes violations immediately visible. This is a CONTRACT, not just documentation.

**Content**: See [allowed-to-use-matrix.md](allowed-to-use-matrix.md) for complete matrix

---

## 2. Change Impact Heatmap

**What it is**: Pre-analyzed scenario matrix showing subsystem impact for common changes.

**Not in C4/arc42**:
- C4 has no impact analysis
- arc42 has risks/debt but not scenario-based prediction

**Innovation**: Proactive blast radius prediction with ✓✓/✓/✗ ratings for 5 common scenarios BEFORE changes are made.

**Content**: See [change-impact-heatmap.md](change-impact-heatmap.md)
