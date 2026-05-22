# Combining `maintainable-code` with other practices

This directory is a **Cursor skill** (`SKILL.md`). The agent loads it when the description matches the task. Use this README for **you**: how to stack this skill with LID, testing, security, and other workflows without conflicting instructions.

## Mental model

| Layer | Role | Typical sources |
|-------|------|-----------------|
| **Structure** | Where volatility lives; how deep modules and seams reduce change cost | This skill (`maintainable-code`) |
| **Process** | When to design, how to review, traceability cadence | LID, Cavekit-style phases, team rituals |
| **Correctness** | Regressions, contracts, confidence to refactor | Tests, types, property-based checks |
| **Production** | Debuggability, safety, SLOs | Observability, security, SRE practices |

`maintainable-code` focuses on the **structure** layer. It stays language-agnostic and does not replace your team’s full SDLC.

## Forensics, lineage, and further reading

- **Parnas → volatility:** The skill’s §1 cites Parnas (1972) on hiding likely-to-change decisions; a longer narrative (including Juval Löwy / iDesign and axes of volatility) is in [Notes and learning on Software Architecture](https://andriynotes.substack.com/p/notes-and-learning-on-software-architecture).
- **Tornhill-style validation:** Use VCS **hotspots** and **change coupling** to check whether your volatility seams match where change really happens (`SKILL.md` §4).
- **Tar Pit / Simple Made Easy:** How accidental complexity, state, and “simple vs easy” relate to this skill is summarized in [reference.md](reference.md) under *Related ideas*.

## With Linked-Intent Development (LID)

**Complement, don’t duplicate.** LID answers: *what documents exist, in what order, and when do we stop for human review?* This skill answers: *how should modules and boundaries look so the system stays evolvable?*

**Suggested division:**

- **LID skill:** HLD → LLD → EARS → implementation plan; brownfield reconnaissance; `@spec` links; arrow status if you use it.
- **`maintainable-code`:** Volatility-based boundaries, information hiding, deep modules, Pike-style data-first thinking, error/contract discipline, and the **change checklist** inside each design or PR.

**Tip:** In LLDs, add an explicit **“volatility”** subsection: what is expected to change, and which module owns each kind of change. That connects Löwy directly to LID’s artifacts.

## With spec- or kit-driven methods (e.g. Cavekit-style)

Use kits or domain specs as the **source of requirements**; use this skill when **mapping requirements to structure**—especially which volatilities get isolated modules and which stay inline.

**Traceability:** If every line traces to a requirement, prefer seams that align with **requirement clusters** that change together, not with UI page order or story sequence alone.

## With testing

- **Before refactors:** Prefer characterization or golden tests so “maintainable” reshaping does not change behavior silently.
- **Design feedback:** If tests need painful setup for every caller, the module boundary is often too shallow or leaky.
- **Contract tests** at integration seams match Ousterhout-style explicit contracts.

This skill does not prescribe TDD; your project’s testing skill or conventions should.

## With ADRs (architecture decision records)

Use ADRs for **decisions that are hard to reverse** or that explain non-obvious trade-offs. Pair with:

- **Volatility:** ADR records *what we optimized for* when multiple volatilities competed.
- **Information hiding:** ADR can state *which decision is encapsulated where* so leakage reviews have a anchor.

## With security and compliance

Structure reduces accidental complexity; **security** reduces adversarial and abuse risk. Combine:

- **Least privilege** and explicit trust boundaries at the same seams you use for volatility.
- **Input validation** at module boundaries that face untrusted callers or networks.

Do not infer security from “clean code” alone.

## With observability and operations

Deep modules simplify **call sites**; observability simplifies **incident response**. At service boundaries, document:

- **Idempotency**, retry safety, and error semantics (ties to “define errors out of existence” where applicable).
- **Signals** (logs/metrics/traces) that prove the contract in production.

## With code review

Use the **change checklist** in `SKILL.md` as a short review pass. For larger reviews, add project-specific checks (performance, accessibility, legal) from your team standards.

## Enabling multiple skills in Cursor

1. Install or symlink this folder under `~/.cursor/skills/maintainable-code/` (personal) or `.cursor/skills/maintainable-code/` (project).
2. Ensure each skill has a clear **description** in frontmatter so the agent picks the right one.
3. When a task needs both process and structure, **name both** in your prompt, e.g. “Follow LID for this feature; apply maintainable-code principles in the LLD and implementation.”

Conflicts are rare if you treat LID as **workflow** and `maintainable-code` as **design principles** inside that workflow.
