# Combining `python-style` with other skills

## With `maintainable-code` (language-agnostic)

| Skill | Question it answers |
|-------|---------------------|
| **`maintainable-code`** | Where should volatility live? Are modules deep? Is information leaking? |
| **`python-style`** | Is this Python idiomatic, typed, and lint-clean? |

Use **both** on the same PR: structure first, then Python hygiene (or iterate between them when refactors touch boundaries).

**Example prompt:** “Apply `maintainable-code` for the new package boundary, then `python-style` for imports, types, and Ruff.”

## With Linked-Intent Development (LID)

LID governs **when** and **what** you document before coding. `python-style` governs **how** the Python reads once you implement. LLDs can reference public API types and exception contracts that this skill helps keep consistent.

## With testing

This skill does not replace tests. Type checkers and Ruff catch different bugs than pytest. Prefer running the project’s test command after substantive edits.

## Retort / LLM language notes

Tools like [Retort](https://github.com/adrianco/retort) compare **stacks** (language × model × tooling) experimentally. Results are **task- and configuration-specific**; they do not replace team standards. Use `python-style` to keep Python consistent regardless of which model generated a draft.
