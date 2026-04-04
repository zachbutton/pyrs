---
name: pyrs-diff
description: "Invoke pyrs-foundation first. Compare code against the pyramid(s) it should mirror and manage per-target unresolved-gap diff sidecars. Triggered by ::diff P where P is an @-prefixed pyramid reference or code reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid Diff

This skill is activated when the user issues `::diff P` where P is an `@`-prefixed pyramid reference or code reference (e.g., `::diff @event-bus`, `::diff @./pyramids/event-bus/index.md`, or `::diff src/event-bus/`). The same behavior also applies when another command performs an explicit same-target post-mutation diff refresh.

## Procedure

1. **Resolve the pyramid** — if P is a pyramid reference, resolve the `@`-prefixed reference to its pyramid file (see Pyramid Identifiers in _foundation.md). If P is a code path, determine which pyramid(s) govern that code.
2. **Read the pyramid(s)** — understand Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Read relevant parent and child pyramids** for broader context
4. **Read the corresponding code** — understand what it actually does and implies
5. **Check for drift** between the pyramid concepts and the code:
   - Does the code implement behavior not described in the pyramid?
   - Does the pyramid describe contracts the code doesn't uphold?
   - Does the code imply relationships or dependencies not captured in the pyramid hierarchy?
   - Are there `PYRS_TODO` placeholders that should have been resolved?
6. **Manage per-target diff sidecars** — for each targeted pyramid, manage a sibling `diff.md` at `./pyramids/path/to/target/diff.md`.
   - Treat `diff.md` as a derived audit artifact, not a pyramid definition.
   - If one or more unresolved discrepancies exist, create or update `diff.md` with evidence-backed findings (pyramid sections and code locations).
   - If zero discrepancies remain, delete any existing `diff.md` for that target.
   - If `P` is a code path that maps to multiple pyramids, apply this lifecycle independently for each target pyramid.
7. **Report findings with evidence** — when drift is found, cite specific pyramids, sections, or code locations. Do not give vague summaries. Then **ask probing questions** about how to reconcile:
    - Update the code to match the pyramid?
    - Update the pyramid to match the code?
    - Both need revision?
8. **Hand off to the change flow** — once the user decides, route them to the appropriate command (`::spec` for pyramid changes, `::apply` for code changes). Do **NOT** perform the resolution directly — the diff command surfaces problems and connects the user to the right command, nothing more.

## Output Constraints

- Diff may create, update, or delete `diff.md` sidecars only in targeted pyramid directories.
- Diff must never modify implementation code or pyramid `index.md` files.
- Diff must never leave empty or no-drift `diff.md` sidecars behind.
- When run as a post-mutation refresh, diff must stay target-scoped and must not broaden into a multi-target sweep unless explicitly requested by the user.

## What Drift Looks Like

- Code that does more than the pyramid describes (scope creep)
- Code that doesn't fulfill a contract stated in the pyramid
- Code with implicit concepts that have no pyramid representation
- Stale `PYRS_TODO` placeholders for pyramids that now exist
- Code coupling that violates a pyramid's Constraints section
- Missing or inaccurate `PYRS` provenance comments — unless the governing pyramid (or any of its ancestors) has a Constraint prohibiting code markers, in which case provenance comments are not expected for that branch

Do not assume drift is acceptable. Surface every discrepancy and let the user decide.
