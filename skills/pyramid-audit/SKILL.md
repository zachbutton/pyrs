---
name: pyramid-audit
description: "Audit a pyramid for conceptual alignment within the hierarchy. Triggered by ::audit P:: where P is a pyramid identifier."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid Audit

This skill is activated when the user issues `::audit P::` where P is a pyramid identifier (e.g., `::audit event-bus::`).

## Procedure

1. **Resolve P** — locate the pyramid file at `./pyramids/P.md` or `./pyramids/P/index.md`
2. **Read P** thoroughly — understand its Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Traverse upward** — read all parent pyramids from P up to `./pyramids/index.md`
4. **Consider the broad ecosystem** — understand how P fits within the full hierarchy
5. **Check for conceptual drift** between P and its parents:
   - Does P's purpose still align with how the parent describes it?
   - Do P's contracts contradict or duplicate parent or sibling contracts?
   - Are all child references in parents present and accurate?
   - Are there orphan pyramids (missing parent links)?
   - Do constraints conflict across levels?
6. **Report findings** — when drift is found, **ask probing questions** about how the user wants to reconcile:
   - Update P?
   - Update one or more parents?
   - Both?

## Strictness

Audits must be strict. The following are all failures — do not silently pass them:

- **Orphan pyramids** — a pyramid exists but its parent does not reference it
- **Missing parent links** — a pyramid does not reference its parent in its Relationships section
- **Conceptual misalignment** — P describes behavior that contradicts or drifts from what its parent says P should be
- **Missing sections** — a pyramid is missing any of the required sections (Purpose, Concepts, Contracts, Relationships, Constraints)
- **Scope violations** — P describes behavior that belongs to a parent or sibling pyramid

Do not infer or assume alignment. If something is ambiguous, flag it and ask.
