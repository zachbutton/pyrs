---
name: pyrs-implement
description: "Implement the concept described by a pyramid using test-driven development. Triggered by ::implement P:: where P is a pyramid identifier."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules — especially the strictness rules. Everything you implement will be scrutinized by auditors and reviewers who enforce those rules strictly.

# Pyramid Implement

This skill is activated when the user issues `::implement P::` where P is a pyramid identifier (e.g., `::implement event-bus::`).

## Procedure

1. **Resolve P** — resolve the dot-delimited identifier to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read P thoroughly** — understand its Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Detect the project's test framework** — use whatever testing tools the project already has
4. **Implement using strict TDD red/green, incrementally:**
   - Pick one aspect of P's concept
   - Write a test that captures that aspect
   - Run the test — confirm it **fails** (red)
   - If the test does not accurately reflect P's concept, revise it before proceeding
   - Write the **minimum code** to make the test **pass** (green)
   - Repeat for the next aspect
5. Do **NOT** write all tests at once. Do **NOT** write all implementation at once. Step-by-step.

## Scope Rules — Critical

These will be enforced by future audits and reviews. Violations are failures.

- Implement **only** what P explicitly describes
- Do **NOT** implement parent pyramid behavior
- Do **NOT** implement child pyramid behavior
- Do **NOT** implement anything not explicitly mentioned in P
- Do **NOT** add features, conveniences, or abstractions beyond P's scope
- If unsure whether something is in scope, it isn't — ask the user

## Unbuilt Dependencies

When P references children or See Also siblings that are not yet built or complete:

- Leave an explicit placeholder comment in the code: `// PRYS_TODO: ./pyramids/[pyramid file or directory]`
- Log the not-implemented behavior meaningfully at runtime so the developer can see it during execution
- Do **NOT** implement the dependency's behavior — only leave the placeholder
- This applies to both child pyramids and sibling concepts referenced via See Also in P's Relationships

## Audit Awareness

Your implementation will be audited against P. Auditors will check:
- That every contract in P is upheld by your code
- That your code does not exceed P's scope
- That relationships and constraints are respected
- That `PRYS_TODO` placeholders exist for unbuilt children and unbuilt See Also dependencies

Write code that cleanly maps to P's concepts. When an auditor reads P and then your code, the alignment should be obvious.
