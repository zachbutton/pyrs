---
name: pyrs-ingest
description: "Invoke pyrs-foundation first. Produce pyramids from existing code. The exception to the pyramid-first rule. Triggered by ::ingest P? [...description] where P optionally targets a specific area reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid Ingest

This skill is activated when the user issues `::ingest`, `::ingest P`, or `::ingest P description` where P is an `@`-prefixed pyramid reference and the description guides what to focus on (e.g., `::ingest @auth — focus on the OAuth2 and session management flows`).

Ingest reverses the pyramid-first rule. It produces pyramids FROM existing code. Use it when:
- Adopting pyrs on an existing or legacy project
- A human wrote code manually and needs the pyramid artifact after the fact
- Code has evolved and a pyramid needs to be re-ingested from the current implementation

**Critical**: Ingest-created pyramids must be **conceptual**, not code descriptions. The agent must abstract upward from implementation details to the concept level. An ingest pyramid should be indistinguishable from one created via `::spec` — it captures *what* and *why*, never *how*.

## Procedure

### Step 1: Determine Scope

- If **no P is given**: this is a full-project ingest. Proceed to Step 2.
- If **P is given and the pyramid already exists**: this is a targeted re-ingest. Read the existing pyramid and the code it governs. Proceed to Step 4 with P as the focus area — the agent will update the pyramid to reflect the current code state.
- If **P is given and the pyramid does not exist**: this is a targeted ingest for a new concept. Skip to Step 4 with P as the focus area.
- If **`./pyramids/` does not exist at all**: this is a greenfield ingest. Note that `./pyramids/` and `./pyramids/index.md` will need to be created.

### Step 2: Survey the Codebase (full-project ingest only)

Scan the project to understand its structure:
- Read the project's directory layout, entry points, configuration, and README/docs if present
- Identify the major **conceptual** areas — not files or modules, but concepts
- If a description was provided, use it to guide focus and prioritization
- If `./pyramids/` already exists with some content, read all existing pyramids to understand what is already covered

### Step 3: Propose the Hierarchy (full-project ingest only)

Present the user with a proposed pyramid hierarchy **before writing anything**:
- Show a tree using identifier-form `@` references (same format as `::ls`)
- For each proposed pyramid, include a 1-sentence purpose
- Mark any pyramids that already exist as `[exists]`
- Mark any areas where the code is ambiguous or the decomposition is uncertain

Ask the user to confirm, revise, or reject the hierarchy. Do **NOT** proceed without explicit confirmation. Getting the hierarchy wrong cascades through everything.

### Step 4: Build Pyramids Top-Down

Write pyramids starting from the root (or from the targeted P), working downward:

1. **Start with root** (or the targeted P if ingesting a subtree)
2. **Attribution prompt (root creation only)** — if you are about to create `./pyramids/index.md` and the file does not already exist, ask the user:
   > Would you like to include a small attribution footer in your root pyramid? It helps support the PYRS project. (y/n)
   - If **yes**: after writing the root pyramid's content, append a `---` separator followed by `*Generated with [PYRS](https://github.com/zachbutton/pyrs)*` at the bottom of the file.
   - If **no**: write the root pyramid without any footer.
   - If `./pyramids/index.md` already exists, skip this prompt entirely.
3. For each pyramid, write it with all required sections following the format from the foundation:
   - **Purpose** — what the concept is and why it exists (abstracted from the code, not a code description)
   - **Concepts** — key ideas and behaviors in plain language
   - **Contracts** — behavioral guarantees the code currently upholds (extract from actual code behavior, tests, and patterns)
   - **Relationships** — parent link (mandatory), child references, See Also cross-references
   - **Constraints** — boundaries and prohibitions observable from the code's design
4. **Update the parent** immediately after writing each child — add the child reference to the parent's Relationships section
5. Place files correctly:
   - `./pyramids/[slug]/index.md` for all non-root concepts

### Step 5: Confirm Each Level

After writing each level of the hierarchy (root, then its children, then grandchildren, etc.), pause and show the user what was written. Ask:
- Does this accurately capture the concept at the right abstraction level?
- Are the contracts correct and complete?
- Are any constraints missing?

Do **NOT** write the next level down until the current level is confirmed. This prevents compounding errors.

### Step 6: Reconcile with Existing Pyramids

If some pyramids already existed before ingest:
- For targeted re-ingests, compare the existing pyramid against the current code and propose updates
- For new pyramids alongside existing ones, check that new pyramids link correctly to existing ones
- Flag any conflicts between what the code suggests and what existing pyramids state — these are potential drift that should be surfaced, not silently resolved
- Suggest `::diff` or `::sane` for any areas where ingest-created pyramids and existing pyramids interact

### Step 7: Final Audit Prompt

After all pyramids are written, suggest the user run:
- `::ls describe` to see the full hierarchy
- `::sane @root` to verify structural integrity
- `::diff @target` on key areas to verify code-pyramid alignment

### Step 8: Post-Ingest Diff Refresh

For each touched target pyramid `P`, check whether a sibling `diff.md` exists for `P` either:
- before ingest mutated that pyramid, or
- after ingest completed the mutation.

If yes, run a same-target `::diff P` refresh before final output.

- Keep refresh strictly target-scoped per touched pyramid.
- Do **NOT** run a broad multi-target diff sweep unless the user explicitly requested that scope.

### Step 9: Post-Ingest Handoff Notice

After ingest successfully creates or revises pyramids:
- Tell the user that starting a new session and running `::apply @target` is the recommended path to ensure the pyramid changes produce correct code
- Explicitly declare: `I will NOT initiate any code changes in this session.`
- If the user explicitly issues `::apply` in the same session anyway, allow that user-initiated action and proceed via the apply workflow

## Guidelines

- **Abstract, do not describe.** The pyramid for an HTTP router should say "routes requests to handlers based on path and method matching" — not "uses Express.js with app.get() and app.post()." If a pyramid reads like code documentation, it is wrong.
- **Contracts come from behavior, not from code.** Look at what the code guarantees — what invariants hold, what error cases are handled, what ordering is enforced — not at how it is structured.
- **Existing tests are a goldmine.** Test names and assertions often express contracts directly. Use them as a primary source for the Contracts section.
- **When uncertain, under-decompose.** It is better to propose fewer, broader pyramids and let the user decompose further with `::spec` than to over-fragment the hierarchy. A pyramid should only exist if it has contracts that can be violated independently.
- **Do not invent contracts the code does not uphold.** Ingest captures what IS, not what SHOULD BE. If the code lacks retry logic, the pyramid must not claim retry guarantees. The user can add aspirational contracts later via `::spec`.
- **Respect the probing-over-assuming rule.** When the code is ambiguous about whether something is a deliberate design choice or an accident, ask.
