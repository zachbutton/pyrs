---
name: pyrs-review
description: "Review code against the pyramid(s) it should mirror. Triggered by ::review P:: where P is a pyramid identifier or code reference."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid Review

This skill is activated when the user issues `::review P::` where P is a pyramid identifier or code reference (e.g., `::review event-bus::` or `::review src/event-bus/`).

## Procedure

1. **Resolve the pyramid** — if P is a pyramid identifier, resolve the dot-delimited identifier to its pyramid file (see Pyramid Identifiers in _foundation.md). If P is a code path, determine which pyramid(s) govern that code.
2. **Read the pyramid(s)** — understand Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Read relevant parent and child pyramids** for broader context
4. **Read the corresponding code** — understand what it actually does and implies
5. **Check for drift** between the pyramid concepts and the code:
   - Does the code implement behavior not described in the pyramid?
   - Does the pyramid describe contracts the code doesn't uphold?
   - Does the code imply relationships or dependencies not captured in the pyramid hierarchy?
   - Are there `PYRS_TODO` placeholders that should have been resolved?
6. **Report findings** — when drift is found, **ask probing questions** about how to reconcile:
   - Update the code to match the pyramid?
   - Update the pyramid to match the code?
   - Both need revision?

## What Drift Looks Like

- Code that does more than the pyramid describes (scope creep)
- Code that doesn't fulfill a contract stated in the pyramid
- Code with implicit concepts that have no pyramid representation
- Stale `PYRS_TODO` placeholders for pyramids that now exist
- Code coupling that violates a pyramid's Constraints section
- Missing or inaccurate `PYRS` provenance comments — unless the governing pyramid (or any of its ancestors) has a Constraint prohibiting code markers, in which case provenance comments are not expected for that branch

Do not assume drift is acceptable. Surface every discrepancy and let the user decide.
