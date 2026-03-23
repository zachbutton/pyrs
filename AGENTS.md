# Pyramid Workflow — Agent Guide

This document instructs LLM agents on how to operate within the **pyramid workflow**, a hierarchical conceptual documentation system for software projects.

## What Pyramids Are

A project using the pyramid workflow has a `./pyramids/` directory containing `.md` files that describe features, product requirements, and relationships between concepts at a **high level**. Pyramids are not code — they are conceptual documentation. Code is implementation detail; pyramids define the *contracts and relationships* between concepts.

### Directory Structure

- `./pyramids/index.md` — the broadest overview of the product
- Nested concepts use **slug-formatted** names:
  - **Directory form** (for concepts with children): `./pyramids/event-bus/index.md`
  - **File form** (for small, self-contained concepts): `./pyramids/event-bus.md`
- Nesting is recursive — deeper levels are more granular but remain high-level conceptual descriptions
- Parent pyramids reference children using slug-formatted markers (e.g., `event-bus` in `index.md` refers to `./pyramids/event-bus/index.md` or `./pyramids/event-bus.md`)

### Content Rules

- Pyramids describe **concepts**, not code
- Code snippets are permitted only as abstract illustrations of a concept — never as prescriptive implementation
- Real code does NOT need to match example code; it only needs to match the **concept**
- If a concept can be explained without code, prefer that

## Pyramid File Structure

Every pyramid file should include the following sections:

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

### Constraints

Boundaries and prohibitions — what this concept must NOT do or become. These guard against scope creep during implementation and audit.

## Commands

Users issue pyramid commands in the format `::command context::` (two colons on each side). The first token is the command; everything after it is context, args, or info.

---

### `::audit P::`

Audit pyramid P for conceptual alignment within the hierarchy.

**Procedure:**
1. Read pyramid P
2. Traverse **upward** — read all parent pyramids up to `./pyramids/index.md`
3. Consider the broad ecosystem and how P fits within it
4. Check for **conceptual drift** between P and its parents:
   - Does P's purpose still align with how the parent describes it?
   - Do P's contracts contradict or duplicate parent/sibling contracts?
   - Are all child references in parents present and accurate?
   - Are there orphan pyramids (missing parent links)?
5. When drift is found, **ask probing questions** about how the user wants to reconcile:
   - Update P?
   - Update one or more parents?
   - Both?

**Strictness:** Audits must be strict. Orphan pyramids, missing links in parents, and conceptual misalignment are all failures. Do not silently pass these.

---

### `::review P::`

Review actual code against the pyramid(s) it should mirror.

**Procedure:**
1. Read pyramid P (and relevant parent/child pyramids for context)
2. Read the corresponding code
3. Check for **drift** between what the pyramids describe conceptually and what the code does or implies
4. When drift is found, **ask probing questions** about how to reconcile:
   - Update the code to match the pyramid?
   - Update the pyramid to match the code?
   - Both need revision?

---

### `::implement P::`

Implement the concept described by pyramid P in code.

**Procedure:**
1. Read pyramid P thoroughly
2. Use **test-driven development (red/green)**, incrementally:
   - Write a test that captures one aspect of P's concept
   - Run the test — confirm it **fails** (red)
   - If the test does not accurately reflect P's concept, revise it
   - Write the minimum code to make the test **pass** (green)
   - Repeat for the next aspect
3. Do **NOT** attempt to write all tests at once or all implementation at once — work step-by-step
4. Use the project's existing test framework (detect from project tooling)

**Scope rules — these are critical:**
- Implement **only** what P explicitly describes
- Do **NOT** implement parent pyramid behavior
- Do **NOT** implement child pyramid behavior
- Do **NOT** implement anything not explicitly mentioned in P
- Violating scope will cause future audit failures

**Unbuilt dependencies:**
When P references children that are not yet built or complete:
- Leave an explicit placeholder comment: `// PRYS_TODO: ./pyramids/[pyramid file or directory]`
- Log the not-implemented behavior meaningfully at runtime so developers can see it

---

### `::tighten P::`

Tighten an existing implementation against an updated pyramid P.

Use this when a pyramid has been revised and the existing code needs to conform to the changes.

**Procedure:**
1. Read the updated pyramid P
2. Read the existing code and tests
3. Use **test-driven development (red/green)**, incrementally:
   - Write or update tests to reflect the **updated** pyramid's concepts and contracts
   - Run the tests — confirm they **fail** against the current code (red)
   - Update the code to make the tests **pass** (green)
   - Repeat for each changed aspect
4. Do **NOT** batch all test changes or all code changes — work step-by-step
5. Same strict scope rules as `::implement::` apply

---

### `::new description::`

Create a new pyramid from a user-provided description or reference.

**Procedure:**
1. Determine from the description:
   - Where the pyramid belongs in the hierarchy
   - What it relates to
   - Whether placeholders (`PRYS_TODO`) or references to it exist in the code
2. If placement is unclear, **ask the user** probing questions
3. Read the draft pyramid's **parent pyramid(s)** to assess conceptual fit
   - If the concept doesn't fit cleanly, ask a second round of probing questions
4. Write the pyramid file with all required sections (Purpose, Concepts, Contracts, Relationships, Constraints)
5. **Update the parent pyramid** to reference the new child
   - Missing parent links produce orphan pyramids, which fail audits

## General Agent Rules

1. **Pyramids are conceptual** — never treat them as code specifications. Code must match the concept, not any example syntax.
2. **Scope is strict** — `::implement::` and `::tighten::` must not exceed the bounds of the specified pyramid. No parent behavior. No child behavior. No undocumented behavior.
3. **Links are mandatory** — every pyramid must be referenced by its parent. Every pyramid must reference its parent. Orphans fail audits.
4. **Audits are strict** — do not pass audits when there is drift, missing links, or scope violations. Surface every issue.
5. **Probing over assuming** — when drift or ambiguity is found during any command, ask the user rather than making assumptions about intent.
6. **Placeholder format** — for unbuilt dependencies: `// PRYS_TODO: ./pyramids/[path]` with runtime logging.
