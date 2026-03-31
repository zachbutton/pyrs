# Pyramid Workflow — Foundation

## What Pyramids Are

A project using the pyramid workflow has a `./pyramids/` directory containing `.md` files that describe features, product requirements, and relationships between concepts at a **high level**. Pyramids are not code — they are operational artifacts that drive implementation and verification. Code is implementation detail; pyramids define the *contracts and relationships* between concepts and serve as the source of truth that precedes code.

## Directory Structure

- `./pyramids/index.md` — the broadest overview of the product, known as `root`
- Nested concepts use **slug-formatted** names:
  - **Directory form** (for concepts with children): `./pyramids/event-bus/index.md`
  - **File form** (for small, self-contained concepts): `./pyramids/event-bus.md`
- Nesting is recursive — deeper levels are more granular but remain high-level conceptual descriptions
- Parent pyramids reference children using slug-formatted markers (e.g., `event-bus` in `index.md` refers to `./pyramids/event-bus/index.md` or `./pyramids/event-bus.md`)

## Pyramid Identifiers

Pyramids can be referenced anywhere using **dot-delimited identifiers**. These resolve to file paths:

- `event-bus` → `./pyramids/event-bus/index.md` or `./pyramids/event-bus.md`
- `event-bus.actions` → `./pyramids/event-bus/actions/index.md` or `./pyramids/event-bus/actions.md`
- `root` → `./pyramids/index.md`
- `root.event-bus.actions` → same as `event-bus.actions`

`root` refers to `./pyramids/index.md` and is **optional** as a prefix — `root.event-bus.actions` and `event-bus.actions` mean the same thing. Use `root` explicitly only when you need to reference `./pyramids/index.md` itself.

These identifiers are used in commands (`::audit event-bus.actions::`), in See Also references, and anywhere a pyramid needs to be referenced by name.

## Content Rules

- Pyramids describe **concepts**, not code
- Code snippets are permitted only as abstract illustrations of a concept — never as prescriptive implementation
- Real code does NOT need to match example code; it only needs to match the **concept**
- If a concept can be explained without code, prefer that

## Pyramid File Sections

Every pyramid file should include these sections:

### Purpose
What the concept is and why it exists.

### Concepts
Key ideas and behaviors, described in plain language. Use abstract code only when it genuinely clarifies a concept that words alone cannot.

### Contracts
Behavioral guarantees and invariants that this concept upholds. These are what audits and reviews check against. Contracts define what must be true, not how it is achieved.

### Relationships
Explicit links to parent and child pyramids. Every pyramid **must** link to its parent, and the parent **must** reference it. A pyramid with no parent link is an **orphan** and will fail audits.

Format:
- Parent: `[Parent Name](../index.md)`
- Children: `[Child Name](./child-slug/index.md)` or `[Child Name](./child-slug.md)`
- See Also: `[Label](../sibling/path.md)` — cross-references to related pyramids that are not parent/child. Use dot-delimited identifiers to reference them (e.g., `event-bus.actions` resolves to `./pyramids/event-bus/actions/index.md` or `./pyramids/event-bus/actions.md`)

### Constraints
Boundaries and prohibitions — what this concept must NOT do or become. These guard against scope creep during implementation and audit.

## Command Syntax

Users issue pyramid commands in the format `::command context::` — two colons on each side. The first token is the command; everything after it is context, args, or info.

## Change Flow

All changes begin in the pyramids, never in the code directly. The pyramid workflow enforces this path:

1. **Describe the change conceptually** — use `::new::` to create a pyramid or `::update::` to revise one
2. **Verify alignment** — use `::audit::` to ensure the change fits the hierarchy
3. **Implement or tighten** — use `::implement::` for new pyramids or `::tighten::` for revised ones
4. **Review for drift** — use `::review::` to verify the code matches the pyramid

This loop runs continuously across sessions.

`::implement::` and `::tighten::` are the **only** routes to code changes. Code must never be modified outside of these commands. If a user describes a change without specifying a pyramid, the appropriate pyramid must be identified first — ask the user to confirm before modifying anything.

**Exception: `::bootstrap::`** — when adopting pyrs on an existing project or capturing pyramids for code a human wrote, `::bootstrap::` reverses the flow: it reads code and produces pyramids. This is the only time code drives pyramid creation. Once bootstrap completes, the normal pyramid-first flow applies to all future changes.

## Strictness Rules

These rules apply across ALL pyramid operations. Violations are audit failures.

1. **Pyramids are conceptual** — never treat them as code specifications. Code must match the concept, not any example syntax.
2. **Scope is strict** — implementation must not exceed the bounds of the specified pyramid. No parent behavior. No child behavior. No undocumented behavior.
3. **Links are mandatory** — every pyramid must be referenced by its parent. Every pyramid must reference its parent. Orphans fail audits.
4. **Audits are strict** — do not pass audits when there is drift, missing links, or scope violations. Surface every issue.
5. **Probing over assuming** — when drift or ambiguity is found, ask the user rather than making assumptions about intent.
6. **Placeholder format** — for unbuilt dependencies (children or See Also siblings): `// PYRS_TODO: ./pyramids/[path]` with meaningful runtime logging. **Exception:** if the pyramid (or any of its ancestors) has a Constraint prohibiting code markers, do not insert placeholders — instead, note unbuilt dependencies in the pyramid itself. This exception is inherited: a parent's opt-out applies to all its descendants, but does not affect other branches of the hierarchy.
7. **Provenance comments** — code and tests generated from a pyramid must include `// PYRS: <identifier>` comments (e.g., `// PYRS: event-bus.actions`) using the pyramid's dot-delimited identifier. Place these at the top of files, on key functions, classes, and test blocks so the connection between code and pyramid is obvious. Use the comment syntax appropriate for the language (`#`, `//`, `/* */`, etc.). Missing or inaccurate provenance comments are audit failures. **Exception:** if the pyramid (or any of its ancestors) has a Constraint prohibiting code markers, omit provenance comments entirely. This exception is inherited: a parent's opt-out applies to all its descendants, but does not affect other branches of the hierarchy.
8. **Git history is not a source of truth** — pyramid operations compare the current pyramid state against the current code state. Do not use `git log`, `git diff`, `git blame`, or any version control history to determine what a pyramid means, what changed, or what to implement. The pyramid file as it exists now is the complete specification.
