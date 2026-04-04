---
name: pyrs-sane
description: "Invoke pyrs-foundation first. Check conceptual alignment by traversing upward from a pyramid to root. Triggered by ::sane P where P is an @-prefixed pyramid reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid Sane

This skill is activated when the user issues `::sane P` where P is an `@`-prefixed pyramid reference (e.g., `::sane @event-bus` or `::sane @./pyramids/event-bus/index.md`).

## Procedure

1. **Resolve P** — resolve the `@`-prefixed pyramid reference to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read P** thoroughly — understand its Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Traverse upward** — read all parent pyramids from P up to `./pyramids/index.md`
4. **Consider the broad ecosystem** — understand how P fits within the full hierarchy
5. **Check for conceptual drift** between P and its parents:
   - Does P's purpose still align with how the parent describes it?
   - Do P's contracts contradict or duplicate parent or sibling contracts?
   - Are all child references in parents present and accurate?
   - Are there orphan pyramids (missing parent links)?
   - Do constraints conflict across levels?
6. **Report findings with evidence** — when drift is found, cite specific pyramids, sections, or code locations. Do not give vague summaries. Then **ask probing questions** about how the user wants to reconcile:
   - Update P?
   - Update one or more parents?
   - Both?
7. **Hand off to the change flow** — once the user decides, route them to the appropriate command (`::spec` for pyramid changes, `::apply` for code changes). Do **NOT** perform the resolution directly — the sane command surfaces problems and connects the user to the right command, nothing more.

## Strictness

Audits must be strict. The following are all failures — do not silently pass them:

- **Orphan pyramids** — a pyramid exists but its parent does not reference it
- **Missing parent links** — a pyramid does not reference its parent in its Relationships section
- **Conceptual misalignment** — P describes behavior that contradicts or drifts from what its parent says P should be
- **Missing sections** — a pyramid is missing any of the required sections (Purpose, Concepts, Contracts, Relationships, Constraints)
- **Scope violations** — P describes behavior that belongs to a parent or sibling pyramid
- **Missing or inaccurate provenance comments** — code governed by P lacks `// PYRS: <identifier>` comments, or comments reference the wrong pyramid (for example, `// PYRS: @event-bus.actions`). Exception: if P (or any of its ancestors) has a Constraint prohibiting code markers, provenance comments are not required for P or its descendants — but all other pyramids and branches still require them.
- **Missing or stale placeholders** — unbuilt dependencies lack `// PYRS_TODO` placeholders, or placeholders exist for dependencies that have since been built. Same code-marker exception as above applies.

Do not infer or assume alignment. If something is ambiguous, flag it and ask.
