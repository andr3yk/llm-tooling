---
name: maintainable-code
description: >-
  Guides language-agnostic design and implementation for long-term changeability: volatility-based boundaries, managing complexity, deep modules, and lean error handling. Use when designing modules or APIs, refactoring, reviewing architecture, reducing coupling, or when the user asks for maintainable or evolvable structure. Do not use for language-specific style formatting or simple syntax fixes.
---

# Maintainable code (language-agnostic)

Execute the following steps chronologically when designing, refactoring, or reviewing code architecture to ensure long-term changeability.

## Step 1: Decompose by volatility

1. Identify what is likely to change (rules, integrations, policies, formats, vendors, UX experiments).
2. Encapsulate each kind of change behind a stable seam.
3. Implement business behavior as interactions between those encapsulated parts.
4. Do not decompose primarily from a flowchart of steps. Name modules around domains of decision, not around scripts.

## Step 2: Design deep modules

1. Create a simple interface with powerful behavior and complexity hidden inside.
2. Ensure that a single design decision is owned by one place. Do not leak the same decision to multiple places.
3. Pull complexity down into the module if it simplifies every caller and keeps the interface small.

## Step 3: Choose data structures first

1. Define data structures and boundaries before writing algorithms.
2. Prefer simple structures unless measurement proves hot paths need more sophistication.

## Step 4: Validate seams with forensics

1. Review version control history to find hotspots (high churn with high complexity).
2. Identify change coupling (files that always change together).
3. If hotspots or change coupling contradict the intended seams, adjust the boundaries.

## Step 5: Define explicit error contracts

1. Define errors out of existence by preferring total APIs and well-defined outcomes over many exceptional cases.
2. Make contracts explicit (preconditions, postconditions, idempotency expectations) for true failures.
3. Maintain backward compatibility for published surfaces (APIs, CLIs, wire formats).

## Step 6: Maintain traceability

1. Link implementation and tests to specs or requirements IDs if the project uses them.
2. Update ADRs or design docs when changing the decision they record.

## Step 7: Complete the change checklist

Before completing the task, verify the following:
1. Is each likely change owned by one module?
2. If this decision changes, how many files must move?
3. Is the public surface smaller than the implementation behind it?
4. Are error and edge-case burdens pushed down to a few well-designed APIs?

## References

- Read `references/reference.md` for lineage, related papers, tensions, and complementary practices.
- Read `references/combining-practices.md` for combining this skill with LID, testing, ops, and security.
