---
name: pyrs-apply
description: "Invoke pyrs-foundation first. Produce or update code from a pyramid using incremental test-driven development. Triggered by ::apply P where P is an @-prefixed pyramid reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules — especially the strictness rules. Everything you implement will be scrutinized by auditors and reviewers who enforce those rules strictly.

# Pyramid Apply

This skill is activated when the user issues `::apply P` where P is an `@`-prefixed pyramid reference (e.g., `::apply @event-bus` or `::apply @./pyramids/event-bus/index.md`).

Apply is the only route from concept to code — it handles both building from scratch and updating existing code to match a revised pyramid.

## Procedure

1. **Resolve P** — resolve the `@`-prefixed pyramid reference to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read P thoroughly** — understand its Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Read existing code and tests** — determine whether code already exists for this pyramid
4. **Determine mode:**
   - If no code exists for P → **Build from scratch** (Step 5a)
   - If code already exists → **Tighten to match** (Step 5b)

### Step 5a: Build from Scratch

1. **Detect the project's test framework** — use whatever testing tools the project already has
2. **Implement using strict TDD red/green, incrementally:**
   - Pick one aspect of P's concept
   - Write a test that captures that aspect
   - Run the test — confirm it **fails** (red)
   - If the test does not accurately reflect P's concept, revise it before proceeding
   - Write the **minimum code** to make the test **pass** (green)
   - Repeat for the next aspect
3. Do **NOT** write all tests at once. Do **NOT** write all implementation at once. Step-by-step.

### Step 5b: Tighten to Match

1. **Compare pyramid against code** — read the pyramid as a complete specification. Anything the pyramid describes that the code doesn't do is missing. Anything the code does that the pyramid doesn't describe is excess. Both are gaps to close.
2. **Tighten using strict TDD red/green, incrementally:**
   - Pick one gap
   - Write or update a test to reflect the **updated** pyramid's concept
   - Run the test — confirm it **fails** against the current code (red)
   - Update the code to make the test **pass** (green)
   - Repeat for the next gap
3. **Remove excess** — code the pyramid no longer describes is removed. It would fail audit as scope creep.
4. Do **NOT** batch all test changes at once. Do **NOT** batch all code changes at once. Step-by-step.

### Step 6: Post-Apply Diff Refresh

After apply mutates code for target `P`, check whether a sibling `diff.md` exists for that target either:
- before mutation started, or
- after mutation completed.

If yes, run a same-target `::diff P` refresh before returning final output.

- Keep this refresh strictly target-scoped.
- Do **NOT** broaden into a multi-target diff sweep unless the user explicitly requested that scope.
- The refresh updates or removes `diff.md` based on remaining discrepancies; apply does not directly edit `diff.md` itself.

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

- Leave an explicit placeholder comment in the code: `// PYRS_TODO: ./pyramids/[pyramid file or directory]`
- Log the not-implemented behavior meaningfully at runtime so the developer can see it during execution
- Do **NOT** implement the dependency's behavior — only leave the placeholder
- This applies to both child pyramids and sibling concepts referenced via See Also in P's Relationships
- When tightening, update or remove stale placeholders for dependencies that have since been built
- **Exception:** if P (or any of its ancestors) has a Constraint prohibiting code markers, do not insert `PYRS_TODO` placeholders — instead, note unbuilt dependencies in the pyramid itself

## Provenance Comments

Mark generated code and tests with `// PYRS: P` comments (using P's identifier-form `@` reference) so the pyramid-to-code mapping is clear. Use the comment syntax appropriate for the language (`#`, `//`, `/* */`, etc.).

- Place a comment at the top of each file created for this pyramid
- Mark key functions, classes, and test blocks that implement P's concepts
- Use the full identifier-form reference (e.g., `// PYRS: @event-bus.actions`), not file paths
- When tightening, update identifiers if code moves between pyramids
- Existing `PYRS` comments from prior runs should remain accurate

These comments help `::diff` trace code back to its governing pyramid and make audits easier.

**Exception:** if P (or any of its ancestors) has a Constraint prohibiting code markers, skip provenance comments entirely. This exception is inherited — a parent's opt-out applies to all its descendants, but does not affect other branches. This is common for meta pyramid repos where the codebase is not owned by the pyramid author.

## Audit Awareness

Your code will be audited against P. Auditors will check:
- That every contract in P is upheld by your code
- That your code does not exceed P's scope
- That relationships and constraints are respected
- That code removed or changed reflects the pyramid's revisions (when tightening)
- That no scope creep was introduced
- That `PYRS_TODO` placeholders are accurate for the current state of children and See Also dependencies (unless P's Constraints prohibit code markers)
- That `PYRS` provenance comments are present and accurate (unless P's Constraints prohibit code markers)

Write code that cleanly maps to P's concepts. When an auditor reads P and then your code, the alignment should be obvious.
