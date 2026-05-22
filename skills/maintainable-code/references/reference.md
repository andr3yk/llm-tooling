# Reference: sources, tensions, and complements

## Author map (what this skill draws from)

| Source | Core contribution |
|--------|-------------------|
| **David Parnas** (1972, *On the Criteria…*) | Decompose modules to hide **difficult** or **likely-to-change** decisions; avoid flowchart-first decomposition—precursor to volatility-based design. |
| **Juval Löwy** (*Righting Software* / iDesign) | Volatility-based decomposition; avoid functional decomposition as primary structure; encapsulate change so the system does not “resonate” with every requirement shift. |
| **John Ousterhout** (*A Philosophy of Software Design*) | Complexity = dependencies + obscurity; deep vs shallow modules; information hiding vs leakage; strategic programming; “define errors out of existence”; design twice. |
| **Rob Pike** (five rules) | Measure before tuning; simple algorithms and data structures; **data dominates**—right structures make algorithms obvious. |
| **Adam Tornhill** (*Your Code as a Crime Scene*) | Forensic view: hotspots, change coupling, temporal and social signals from history—**validate** design hypotheses. |
| **Linked-Intent Development (LID)** | Durable chain from design to code: HLD/LLD/requirements/plan/tests; documentation as living truth in spec-driven shops; `@spec`-style traceability where used. |
| **Cavekit-style methodology** | Spec layer, phased build, validation gates, adversarial review—emphasizes spec–code traceability and parallel execution of independent work. |

### Note on Parnas and Löwy

A readable narrative connecting Parnas, volatility, and practice (including axes of volatility and requirements vs volatilities) appears in [Notes and learning on Software Architecture](https://andriynotes.substack.com/p/notes-and-learning-on-software-architecture) (Substack). Additional iDesign-oriented write-ups include [Software Architecture (Code with Spoon)](https://codewithspoon.com/2017/07/software-architecture/).

This skill **distills** these into agent-facing rules. It is not a substitute for the books or full LID/Cavekit workflows.

## Related ideas (papers and talks)

| Work | Relation to this skill |
|------|-------------------------|
| *Out of the Tar Pit* (Moseley & Marks); [readable summary](http://kmdouglass.github.io/posts/summary-out-of-the-tar-pit/) | Argues much complexity is **accidental**; favors taming state and separating **essential** complexity from incidental—aligns with reducing obscurity and strict boundaries. This skill does **not** mandate a full FP style; use the paper to sharpen **what** to isolate, not as a stack prescription. |
| **Simple Made Easy** (Rich Hickey; [transcript](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/SimpleMadeEasy.md)) | Distinguishes **simple** (one role, composable) from **easy** (familiar). Reinforces deep modules, narrow concepts per seam, and not conflating “comfortable patterns” with good structure. |

## Personal heuristics (optional; project-dependent)

These rows are **working notes**: plausible tactics aligned with the rest of this reference, **not** outcomes from controlled experiments or mandatory rules. Validate in your codebase (including Tornhill-style history: do these choices actually reduce churn and coupling where you work?).

| Do more of | Why | Caveat |
|------------|-----|--------|
| **Dependency injection** (constructor/setter injection, explicit wiring) | Keeps seams testable and avoids hard-coded singletons at boundaries. | In scripts and tiny CLIs, a full DI graph can be **overkill**; use judgment. |
| **Explicit data models** (DTOs, schemas, e.g. Pydantic-style in Python) | Makes contracts and validation visible at boundaries. | Watch for **model duplication** (API vs domain); map layers deliberately so volatility does not leak as three parallel types everywhere. |
| **Strong data structures** | Same as Pike: structure carries design. | — |
| **Disciplined exceptions** | Specific types, clear propagation, avoid control-flow by catch-all. | — |
| **Tests (including characterization before refactors)** | Safety net when volatility or forensics say “touch here.” | TDD is a **means**, not proof of good architecture—cheap tests can still lock in bad seams. |
| **Refactor without attachment** | Structure is a hypothesis; history may disprove it. | — |
| **Know your language primitives** | Avoid fake “patterns” that fight the runtime. | — |

| Use sparingly / question | Why | Caveat |
|--------------------------|-----|--------|
| **Deep class inheritance** for **domain** modeling | Tall `is-a` trees often obscure volatilities; composition and small interfaces often read clearer. | **Framework and UI toolkits** often *expect* inheritance (e.g. `ModelView`, widget bases)—idiom wins over dogma there. |

| Neutral | Notes |
|---------|--------|
| **Abstract class vs interface** | Language-specific (e.g. Java/C# vs Python protocols); choose per idioms and test needs. |

**Related workflows (not “orthogonal” to good software, but out of scope for *this* skill’s core):** **Authorization** and other security policies are often their **own volatility**—they deserve explicit seams, not an afterthought. **Martin Fowler’s *Refactoring*** is the **mechanics** of safe change; this skill is **where** to change. Use both together; neither replaces the other.

## Tensions to resolve in real projects

**Documentation-first vs code-first.** LID’s discipline is “intent upstream of code.” Many teams ship from code plus **ADRs** and tests. This skill requires **consistent traceability** (intent ↔ code ↔ tests), not a single storage format. Prefer whatever the repo already uses.

**Deep modules vs thin layers everywhere.** “Deep” does not mean “god object.” It means **rich behavior behind a small face**. Shallow pass-through wrappers that add no decision-hiding are often worse than one honest module.

**Simple vs sufficient.** Pike’s simplicity is about **avoiding needless cleverness**, not skipping necessary domain complexity. Put unavoidable complexity **behind** clear seams.

**Volatility seams vs over-abstraction.** Not every change deserves a plugin framework. Encode **real** volatility; avoid speculative generality (YAGNI still applies).

**Forensics vs greenfield.** History is thin in new repos; rely more on volatility analysis and design twice until VCS signals exist.

## Complementary practices (not duplicated in SKILL.md)

Worth combining with this skill; see [README.md](README.md) for how.

| Practice | Why it matters |
|----------|----------------|
| **Testing discipline** | Fast feedback; characterization tests before refactors; tests force testable boundaries. |
| **Public API / compatibility** | Versioning, deprecation windows, backward compatibility for published contracts. |
| **Observability** | Structured logs, metrics, traces—maintainability in production, not only in the editor. |
| **Security and trust boundaries** | Least privilege, validation at boundaries, threat modeling—orthogonal to “clean structure” but essential for longevity. |
| **Operational semantics** | Idempotency, retries, timeouts, failure modes at integration points. |
| **ADRs** | Lightweight record of architectural decisions; pairs well with volatility thinking (“what decision are we freezing?”). |
| **Domain-Driven Design (lightweight)** | Bounded contexts, ubiquitous language—strengthens naming and module boundaries alongside Löwy-style domain seams. |
| **Refactoring discipline** | Small steps, keep tests green, strangler patterns for legacy—safe change mechanics. |
| **Conway-aligned ownership** | Module boundaries that match team ownership reduce coordination drag. |

## SOLID (optional bridge)

SOLID overlaps partially with **narrow interfaces** and **single ownership of decisions** (Ousterhout-style). Use it as a **familiar vocabulary**, not as a checklist that forces OOP ceremony in every language.

## Agent-specific engineering

Tiered “agent primitives” (tool registries, trust tiers, token budgets) from some foundational lists are **platform concerns**. Apply them when building agent harnesses; they are not required for general application maintainability.
