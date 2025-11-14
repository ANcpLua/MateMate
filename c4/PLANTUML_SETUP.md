# PlantUML C4 Setup

## Status: ✅ Working

C4 diagrams use PlantUML's standard library syntax:

```
!include <C4/C4_Context>
!include <C4/C4_Container>
```

## Requirements

**Installed via Homebrew:**
- PlantUML 1.2025.10
- Graphviz 14.0.2

**Verify:**
```bash
plantuml -version  # Should show 1.2025.10
dot -V             # Should show 14.0.2
```

## Troubleshooting

**Diagrams don't render:**
1. Check PlantUML is in PATH: `which plantuml`
2. Verify Graphviz: `which dot`
3. Restart Rider

**"Cannot include <C4/...>" error:**

Rider is using old embedded PlantUML. Configure manually:
1. **Rider → Settings → Languages & Frameworks → PlantUML**
2. Set executable path: `/opt/homebrew/bin/plantuml`
3. Test and save

**Preview errors in this file:**

Normal - this is a documentation file with example snippets. Your actual diagrams (c1-system-context.md, c2-container-view.md) work fine.

## References

- [C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML)
- [C4 Model](https://c4model.com/)
