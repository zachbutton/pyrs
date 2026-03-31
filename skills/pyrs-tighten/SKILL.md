---
name: pyrs-tighten
description: "Tighten an existing implementation against an updated pyramid using test-driven development. Triggered by ::tighten P:: where P is a pyramid identifier."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules — especially the strictness rules. Everything you change will be scrutinized by auditors and reviewers who enforce those rules strictly.

# Pyramid Tighten

This skill is activated when the user issues `::tighten P::` where P is a pyramid identifier (e.g., `::tighten event-bus::`).

Use this when a pyramid has been revised and the existing implementation needs to conform to the changes.

## Procedure

1. **Resolve P** — resolve the dot-delimited identifier to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read the updated pyramid P** — understand what changed in Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Read the existing code and tests** — understand what currently exists
4. **Compare pyramid against code** — read the pyramid as a complete specification. Anything the pyramid describes that the code doesn't do is missing. Anything the code does that the pyramid doesn't describe is excess. Both are gaps to close.
5. **Tighten using strict TDD red/green, incrementally:**
   - Pick one changed aspect
   - Write or update a test to reflect the **updated** pyramid's concept
   - Run the test — confirm it **fails** against the current code (red)
   - Update the code to make the test **pass** (green)
   - Repeat for the next changed aspect
6. Do **NOT** batch all test changes at once. Do **NOT** batch all code changes at once. Step-by-step.

## Scope Rules — Critical

Same strict scope rules as `::implement::`:

- Change **only** what the updated pyramid requires
- Do **NOT** implement parent pyramid behavior
- Do **NOT** implement child pyramid behavior
- Do **NOT** add anything not explicitly in the updated P
- Remove code that the updated pyramid no longer describes (it would fail audit as scope creep)

## Unbuilt Dependencies

When tightening reveals new unbuilt dependencies (children or See Also siblings that are not yet built or complete):

- Leave an explicit placeholder comment in the code: `// PYRS_TODO: ./pyramids/[pyramid file or directory]`
- Log the not-implemented behavior meaningfully at runtime so the developer can see it during execution
- Do **NOT** implement the dependency's behavior — only leave the placeholder
- Update or remove stale placeholders for dependencies that have since been built
- **Exception:** if P (or any of its ancestors) has a Constraint prohibiting code markers, do not insert `PYRS_TODO` placeholders — instead, note unbuilt dependencies in the pyramid itself

## Provenance Comments

Maintain `// PYRS: P` comments (using P's dot-delimited identifier) on code and tests touched during tightening. Use the comment syntax appropriate for the language (`#`, `//`, `/* */`, etc.).

- Add provenance comments to any new code or tests written during tightening
- Update identifiers if code moves between pyramids
- Existing `PYRS` comments from prior `::implement::` runs should remain accurate

These comments help `::review::` trace code back to its governing pyramid.

**Exception:** if P (or any of its ancestors) has a Constraint prohibiting code markers, skip provenance comments entirely. This exception is inherited — a parent's opt-out applies to all its descendants, but does not affect other branches. This is common for meta pyramid repos where the codebase is not owned by the pyramid author.

## Audit Awareness

After tightening, your code will be audited against the updated P. Auditors will check:
- That every contract in the updated P is upheld
- That code removed or changed reflects the pyramid's revisions
- That no scope creep was introduced during tightening
- That `PYRS_TODO` placeholders are accurate for the current state of children and See Also dependencies (unless P's Constraints prohibit code markers)
- That `PYRS` provenance comments are present and accurate (unless P's Constraints prohibit code markers)
