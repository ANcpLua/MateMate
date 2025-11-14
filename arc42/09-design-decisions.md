# arc42 Chapter 9: Architecture Design Decisions

## Purpose

This chapter documents significant architectural decisions using Architecture Decision Records (ADRs), capturing the
context, decision, consequences, and alternatives considered.

---

## ADR-001: Blood Type Architecture (T/A/0)

**Date:** 2025-10-01
**Status:** ✅ Accepted
**Deciders:** Architecture Team

### Context

Chess application must support technology evolution (rendering engines, input methods) while maintaining stable chess
logic (FIDE rules rarely change). Without clear separation, changing rendering technology requires touching chess logic,
increasing risk.

### Decision

Implement **Blood Type Architecture** with three classifications:

- **TYPE T (Technical):** K1 (InputAdapter), K2 (RenderingEngine)
    - **Change driver:** Technology evolution (OS APIs, graphics libraries)
    - **Examples:** OpenGL → Vulkan, mouse → touch

- **TYPE A (Application):** K3 (InteractionController), K4 (AnalysisService)
    - **Change driver:** Business rules (game flow, chess rules)
    - **Examples:** Add time controls, support chess variants

- **TYPE 0 (Core):** K5 (PositionStore)
    - **Change driver:** Universal concepts (rarely change)
    - **Examples:** Board representation, move history

**Dependency Rules:**

- TYPE T MUST NOT depend on TYPE A or TYPE 0
- TYPE A MAY depend on TYPE T and TYPE 0
- TYPE 0 MUST NOT depend on anything

### Consequences

#### Positive

- ✅ **Technology independence:** Can replace K2 (rendering) without touching K4 (chess logic)
- ✅ **Testable:** Can test K4 without K1/K2 (no graphics/input needed)
- ✅ **Parallel development:** Frontend team works on K1/K2, backend on K4/K5
- ✅ **Change isolation:** Renderer swap affects only K2 + K3 (2 of 5 subsystems)

#### Negative

- ❌ **Learning curve:** Developers must understand blood type rules
- ❌ **Discipline required:** Easy to accidentally violate (e.g., K2 calling K4 directly)
- ❌ **More interfaces:** K3 acts as mediator, requires additional coordination code

#### Risks

- 🟡 **Medium:** New developers violate rules unknowingly
    - **Mitigation:** Automated dependency checking, code reviews

### Alternatives Considered

**1. Layered Architecture (UI → Logic → Data)**

- **Rejected:** Layers don't enforce stability direction
- **Problem:** UI layer can still call logic directly, no change isolation

**2. Monolithic (No separation)**

- **Rejected:** Impossible to evolve independently
- **Problem:** Rendering change requires full regression testing of chess logic

**3. Microservices (Separate processes)**

- **Rejected:** Overkill for desktop app
- **Problem:** IPC overhead, deployment complexity

---

## ADR-002: FEN Standard for Position Representation

**Date:** 2025-10-05
**Status:** ✅ Accepted
**Deciders:** Tech Lead

### Context

Need canonical format for representing chess positions for:

- Storage in K5 (PositionStore)
- Serialization (save/load games)
- Debugging (human-readable positions)
- Interoperability (export to other engines)

### Decision

Use **Forsyth-Edwards Notation (FEN)** as the canonical position format in K5.

**Example:**

```
rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1
```

**Format:** `<pieces> <active> <castling> <enPassant> <halfmove> <fullmove>`

### Consequences

#### Positive

- ✅ **Industry standard:** Compatible with Stockfish, Lichess, Chess.com
- ✅ **Human-readable:** Easy to debug positions manually
- ✅ **Compact:** ~80 bytes per position (vs ~300 bytes for JSON)
- ✅ **Complete:** Captures all game state (pieces + metadata)
- ✅ **Tested:** 40+ years of usage, edge cases known

#### Negative

- ❌ **Parsing overhead:** ~5ms to parse FEN to internal representation
    - **Mitigation:** Cache parsed positions, parse only on state change
- ❌ **Not JSON:** Cannot use standard JSON libraries
    - **Acceptable:** FEN is simpler and more compact than JSON

#### Risks

- 🟢 **Low:** Standard is stable, no changes expected
- 🟢 **Low:** All chess tools support FEN

### Alternatives Considered

**1. Bitboards (64-bit integers)**

```csharp
ulong whitePawns = 0x000000000000FF00;
ulong blackPawns = 0x00FF000000000000;
```

- **Rejected:** Premature optimization, harder to debug
- **When useful:** If performance profiling shows position representation is bottleneck

**2. Custom JSON**

```json
{
  "board": [[null, null, ...], ...],
  "activePlayer": "white",
  "castling": {"whiteKingside": true, ...}
}
```

- **Rejected:** Reinventing wheel, verbose (~300 bytes vs ~80 bytes)
- **No advantages:** FEN already solves this problem

**3. Database schema (relational)**

```sql
CREATE TABLE positions (
  id INT,
  piece VARCHAR(10),
  square VARCHAR(2),
  ...
)
```

- **Rejected:** Overcomplicated for in-memory state
- **When useful:** If persisting millions of positions (not needed for desktop app)

---

## ADR-003: Unified AnalysisService (K4)

**Date:** 2025-10-10
**Status:** ✅ Accepted
**Deciders:** Tech Lead, Senior Developer

### Context

Chess logic can be decomposed into multiple services:

- Move validation (is move legal?)
- Check detection (is king in check?)
- Checkmate detection (are there legal moves?)
- Position evaluation (who is winning?)
- Best move calculation (computer opponent)

**Question:** Should these be separate subsystems or unified?

### Decision

Unify all chess logic in **K4: AnalysisService** as a single subsystem with one API.

**Interface:**

```csharp
interface IAnalysisService
{
    List<Square> GetLegalMoves(Square square, Position position);
    bool IsMoveLegal(Move move, Position position);
    bool IsKingInCheck(Color color, Position position);
    bool IsCheckmate(Color color, Position position);
    int EvaluatePosition(Position position); // centipawns
    Move GetBestMove(Position position, int depth);
}
```

### Consequences

#### Positive

- ✅ **Single point of truth:** All FIDE rules in one place
- ✅ **Easier testing:** One test suite for all chess logic
- ✅ **Simpler API:** K3 calls one service, not five
- ✅ **FIDE compliance:** Easier to verify all rules together
- ✅ **Cohesive:** All methods share chess rule knowledge

#### Negative

- ❌ **Larger subsystem:** K4 is 2,500 LOC (largest subsystem)
    - **Mitigation:** Internal modules for readability (MoveGenerator, Evaluator, etc.)
- ❌ **Multiple responsibilities:** Violates SRP purist interpretation
    - **Acceptable:** All responsibilities relate to chess rules (high cohesion)

#### Risks

- 🟡 **Medium:** K4 complexity grows over time
    - **Mitigation:** Extract `IAnalysisService` interface if needed (see Technical Debt TD-001)

### Alternatives Considered

**1. Separate subsystems**

```
K4a: MoveValidator
K4b: CheckDetector
K4c: Evaluator
K4d: SearchEngine
```

- **Rejected:** Creates coordination complexity
- **Problem:** CheckDetector needs MoveValidator, Evaluator needs CheckDetector, etc.
    - Results in circular dependencies or deep call chains
- **When useful:** If K4 exceeds 10,000 LOC (currently 2,500 LOC)

**2. Embed in K3 (InteractionController)**

```
K3 contains both UI logic AND chess rules
```

- **Rejected:** Violates blood type separation
- **Problem:** TYPE A (K3) would contain rules, can't test without UI

**3. External chess engine (Stockfish)**

```
K4 = Wrapper around Stockfish process
```

- **Rejected:** Overkill for FIDE rule validation
- **When useful:** If implementing strong computer opponent (not in requirements)

---

## ADR-004: Single-Threaded Event Loop

**Date:** 2025-10-12
**Status:** ✅ Accepted
**Deciders:** Tech Lead

### Context

Desktop chess application must be responsive to user input while performing move validation and rendering. Concurrency
options:

- Multi-threaded (UI thread + worker threads)
- Single-threaded event loop
- Async/await

### Decision

Use **single-threaded event loop** (traditional game loop pattern).

**Architecture:**

```csharp
while (running)
{
    var events = K1.PollInputEvents();       // ~2ms
    foreach (var evt in events)
        K3.HandleEvent(evt);                 // ~20ms

    K2.RenderFrame(K5.GetCurrentPosition()); // ~12ms
    Thread.Sleep(TargetFrameTime - elapsed); // Maintain 60 FPS
}
```

### Consequences

#### Positive

- ✅ **Simplicity:** No locks, mutexes, or race conditions
- ✅ **Deterministic:** Same input always produces same output
- ✅ **Debuggable:** Single call stack, easy to trace
- ✅ **Sufficient:** All operations complete in < 50ms (well below the 100ms P95 move budget)

#### Negative

- ❌ **Blocking:** Long K4 analysis blocks rendering
    - **Mitigation:** Limit K4 analysis to < 100ms per move
- ❌ **No parallelism:** Cannot use multiple CPU cores
    - **Acceptable:** CPU is not bottleneck (K4 analysis is 40-80ms, plenty of headroom)

#### Risks

- 🟢 **Low:** Performance requirements met with single thread
- 🟢 **Low:** No concurrency bugs possible

### Alternatives Considered

**1. Multi-threaded**

```csharp
Thread uiThread = new Thread(() => {
    while (running)
        K2.RenderFrame();
});

Thread logicThread = new Thread(() => {
    while (running)
        K4.AnalyzePosition();
});
```

- **Rejected:** Complexity not justified by performance needs
- **Problem:** Shared state (K5) requires locks, introduces race conditions
- **When useful:** If K4 analysis exceeds 100ms (currently 40-80ms)

**2. Async/await**

```csharp
async Task RunGameLoop()
{
    while (running)
    {
        await K3.HandleEventsAsync();
        await K2.RenderFrameAsync();
    }
}
```

- **Rejected:** Doesn't improve performance for CPU-bound work
- **Problem:** `async` is for I/O-bound work (network, disk), not CPU-bound (chess logic)
- **When useful:** If integrating online play (network I/O)

---

## ADR-005: Zero External Dependencies

**Date:** 2025-10-15
**Status:** ✅ Accepted
**Deciders:** Architecture Team

### Context

Chess application could use external libraries for:

- Chess engine (Stockfish, Leela)
- GUI framework (WPF, Avalonia)
- Graphics (MonoGame, Unity)
- FEN parsing (Chess.NET)

### Decision

Implement all functionality using **.NET Base Class Library only** (zero external NuGet packages).

**Reasoning:**

- MateMate is educational project demonstrating architectural principles
- External dependencies obscure architecture decisions
- Full control over implementation enables blood type enforcement

### Consequences

#### Positive

- ✅ **Zero dependency risk:** No breaking changes from external libraries
- ✅ **Portable:** Runs anywhere .NET 10 runs
- ✅ **Educational clarity:** All code is visible and understandable
- ✅ **Deployment simplicity:** Single executable, no DLL hell

#### Negative

- ❌ **More code to write:** Must implement FEN parsing, rendering, etc.
    - **Acceptable:** Total codebase still < 6,000 LOC (small)
- ❌ **No advanced features:** No 3D graphics, online play, etc.
    - **Acceptable:** Not in requirements

#### Risks

- 🟢 **Low:** .NET BCL is stable and well-documented
- 🟢 **Low:** No security vulnerabilities from dependencies

### Alternatives Considered

**1. Chess.NET (FEN parsing + move generation)**

- **Rejected:** Hides K4 implementation details
- **Problem:** Cannot demonstrate blood type architecture if chess logic is external

**2. MonoGame (Graphics framework)**

- **Rejected:** K2 would be tightly coupled to MonoGame API
- **Problem:** Cannot demonstrate TYPE T replaceability

**3. Stockfish (Chess engine)**

- **Rejected:** K4 would be external process
- **Problem:** IPC overhead, deployment complexity, defeats educational purpose

---

## Decision Summary Table

| ADR     | Decision                   | Primary Benefit       | Trade-off                    |
|---------|----------------------------|-----------------------|------------------------------|
| **001** | Blood Type Architecture    | Change isolation      | Learning curve               |
| **002** | FEN Standard               | Interoperability      | Parsing overhead (~5ms)      |
| **003** | Unified AnalysisService    | Single point of truth | Larger subsystem (2,500 LOC) |
| **004** | Single-threaded Event Loop | Simplicity            | No multi-core parallelism    |
| **005** | Zero External Dependencies | Full control          | More implementation work     |

---

## Technology Radar

Technologies we're **adopting**, **trialing**, **assessing**, or **holding**.

### Adopt (Production-ready)

- ✅ **.NET 10** - Long-term support, stable
- ✅ **FEN Standard** - 40+ years proven
- ✅ **C# Language** - Mature, well-documented

### Trial (Experimenting)

- 🧪 **Blood Type Architecture** - New pattern, proving effectiveness in MateMate
- 🧪 **Allowed-to-Use Matrix** - Governance extension, testing automated checks

### Assess (Watching)

- 👁️ **Blazor** - Could enable web version of MateMate
- 👁️ **WASM** - Potential for browser deployment

### Hold (Avoiding)

- ⛔ **WPF** - Legacy Windows-only framework
- ⛔ **Stockfish Integration** - Unnecessary complexity for educational project

---

## Summary

**5 Key Decisions:**

1. **Blood Type Architecture** - Change driver classification (T/A/0)
2. **FEN Standard** - Industry-standard position format
3. **Unified AnalysisService** - All chess logic in K4
4. **Single-threaded Event Loop** - Simplicity over parallelism
5. **Zero Dependencies** - Full control, educational clarity

All decisions prioritize:

- **Architectural clarity** over feature richness
- **Maintainability** over performance optimization
- **Educational value** over production polish

**Result:** Architecture that demonstrates principles clearly while remaining practical.
