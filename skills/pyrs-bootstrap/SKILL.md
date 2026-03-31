---
name: pyrs-bootstrap
description: "Produce pyramids from existing code. The one exception to the pyramid-first rule. Triggered by ::bootstrap P? [...description]:: where P optionally targets a specific area."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid Bootstrap

This skill is activated when the user issues `::bootstrap::`, `::bootstrap P::`, or `::bootstrap P description::` where P is a pyramid identifier and the description guides what to focus on (e.g., `::bootstrap auth — focus on the OAuth2 and session management flows::`).

Bootstrap is the **one exception** to the pyramid-first rule. It produces pyramids FROM existing code. Use it when:
- Adopting pyrs on an existing or legacy project for the first time
- A human wrote code manually and needs the pyramid artifact after the fact

**Critical**: Bootstrap-created pyramids must be **conceptual**, not code descriptions. The agent must abstract upward from implementation details to the concept level. A bootstrap pyramid should be indistinguishable from one created via `::new::` — it captures *what* and *why*, never *how*.

## Procedure

### Step 1: Determine Scope

- If **no P is given**: this is a full-project bootstrap. Proceed to Step 2.
- If **P is given and the pyramid already exists**: STOP. Tell the user this pyramid already exists. Suggest `::update::` or `::review::` instead.
- If **P is given and the pyramid does not exist**: this is a targeted bootstrap. Skip to Step 4 with P as the focus area.
- If **`./pyramids/` does not exist at all**: this is a greenfield bootstrap. Note that `./pyramids/` and `./pyramids/index.md` will need to be created.

### Step 2: Survey the Codebase (full-project bootstrap only)

Scan the project to understand its structure:
- Read the project's directory layout, entry points, configuration, and README/docs if present
- Identify the major **conceptual** areas — not files or modules, but concepts
- If a description was provided, use it to guide focus and prioritization
- If `./pyramids/` already exists with some content, read all existing pyramids to understand what is already covered

### Step 3: Propose the Hierarchy (full-project bootstrap only)

Present the user with a proposed pyramid hierarchy **before writing anything**:
- Show a tree using dot-delimited identifiers (same format as `::ls::`)
- For each proposed pyramid, include a 1-sentence purpose
- Mark any pyramids that already exist as `[exists]`
- Mark any areas where the code is ambiguous or the decomposition is uncertain

Ask the user to confirm, revise, or reject the hierarchy. Do **NOT** proceed without explicit confirmation. Getting the hierarchy wrong cascades through everything.

### Step 4: Build Pyramids Top-Down

Write pyramids starting from the root (or from the targeted P), working downward:

1. **Start with root** (or the targeted P if bootstrapping a subtree)
2. For each pyramid, write it with all required sections following the format from `_foundation.md`:
   - **Purpose** — what the concept is and why it exists (abstracted from the code, not a code description)
   - **Concepts** — key ideas and behaviors in plain language
   - **Contracts** — behavioral guarantees the code currently upholds (extract from actual code behavior, tests, and patterns)
   - **Relationships** — parent link (mandatory), child references, See Also cross-references
   - **Constraints** — boundaries and prohibitions observable from the code's design
3. **Update the parent** immediately after writing each child — add the child reference to the parent's Relationships section
4. Place files correctly:
   - `./pyramids/[slug]/index.md` for concepts that will have children
   - `./pyramids/[slug].md` for small, self-contained concepts

### Step 5: Confirm Each Level

After writing each level of the hierarchy (root, then its children, then grandchildren, etc.), pause and show the user what was written. Ask:
- Does this accurately capture the concept at the right abstraction level?
- Are the contracts correct and complete?
- Are any constraints missing?

Do **NOT** write the next level down until the current level is confirmed. This prevents compounding errors.

### Step 6: Reconcile with Existing Pyramids

If some pyramids already existed before bootstrap:
- Do **NOT** overwrite them
- Check that new pyramids link correctly to existing ones
- Flag any conflicts between what the code suggests and what existing pyramids state — these are potential drift that should be surfaced, not silently resolved
- Suggest `::review::` or `::audit::` for any areas where bootstrap-created pyramids and existing pyramids interact

### Step 7: Final Audit Prompt

After all pyramids are written, suggest the user run:
- `::ls describe::` to see the full hierarchy
- `::audit root::` to verify structural integrity
- `::review P::` on key areas to verify code-pyramid alignment

## Guidelines

- **Abstract, do not describe.** The pyramid for an HTTP router should say "routes requests to handlers based on path and method matching" — not "uses Express.js with app.get() and app.post()." If a pyramid reads like code documentation, it is wrong.
- **Contracts come from behavior, not from code.** Look at what the code guarantees — what invariants hold, what error cases are handled, what ordering is enforced — not at how it is structured.
- **Existing tests are a goldmine.** Test names and assertions often express contracts directly. Use them as a primary source for the Contracts section.
- **When uncertain, under-decompose.** It is better to propose fewer, broader pyramids and let the user decompose further with `::new::` than to over-fragment the hierarchy. A pyramid should only exist if it has contracts that can be violated independently.
- **Do not invent contracts the code does not uphold.** Bootstrap captures what IS, not what SHOULD BE. If the code lacks retry logic, the pyramid must not claim retry guarantees. The user can add aspirational contracts later via `::update::`.
- **Respect the probing-over-assuming rule.** When the code is ambiguous about whether something is a deliberate design choice or an accident, ask.
