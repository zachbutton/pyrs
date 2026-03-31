---
name: pyrs-survey
description: "Survey a pyramid and its full lineage for structural gaps, under-specified areas, and missing leaves. Triggered by ::survey P:: where P is a pyramid identifier."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid Survey

This skill is activated when the user issues `::survey P::` where P is a pyramid identifier (e.g., `::survey task-queue::`).

## Procedure

1. **Resolve P** — resolve the dot-delimited identifier to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read P** thoroughly — understand its Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Traverse upward** — read all ancestor pyramids from P up to root pyramid at `./pyramids/index.md`
4. **Traverse downward** — read all descendant pyramids of P, recursively
5. **Analyze for structural gaps** across the full lineage:
   - **Missing leaves** — concepts, contracts, or behaviors described in a pyramid that imply a child pyramid should exist but doesn't
   - **Under-specified pyramids** — pyramids with many concepts or contracts that would benefit from decomposition into children
   - **Cross-cutting concerns** — similar concepts appearing in multiple pyramids that might warrant their own shared pyramid
   - **Stale `PYRS_TODO` placeholders** — search the codebase for `PYRS_TODO` references to pyramids that don't exist yet
   - **Implied but unnamed concepts** — behaviors referenced in Constraints or Contracts that aren't captured anywhere in the hierarchy
6. **Present findings** — for each gap, explain:
   - What's missing and where you found the evidence
   - Why it matters (what could go wrong without it)
   - A suggested pyramid name and where it would fit in the hierarchy
7. **Ask the user** which gaps they want to address — do not create pyramids without confirmation
8. **Hand off to the change flow** — once the user decides which gaps to address, route them to the appropriate command (`::new::` for missing pyramids, `::update::` for under-specified ones). Do **NOT** create or modify pyramids directly — the survey command surfaces gaps and connects the user to the right command, nothing more.

## Guidelines

- Focus on structural completeness, not code. This is a pyramid-only analysis.
- Not every concept needs its own pyramid. Only suggest new leaves when the gap is meaningful — when missing it would lead to ambiguity, scope leaks, or unspecified behavior.
- Prioritize findings by impact. Lead with the gaps most likely to cause problems.
- Do not suggest pyramids for implementation details. Pyramids are conceptual — if something is purely a "how," it doesn't need a pyramid.
