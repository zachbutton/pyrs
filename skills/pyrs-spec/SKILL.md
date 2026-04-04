---
name: pyrs-spec
description: "Invoke pyrs-foundation first. Create a new pyramid or revise an existing one. Triggered by ::spec P? [...description] where P optionally targets a specific pyramid reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid Spec

This skill is activated when the user issues `::spec P? [...description]` where P is an optional `@`-prefixed pyramid reference (identifier form like `@event-bus` or direct-link form like `@./pyramids/event-bus/index.md`) and the description explains the new concept or what needs to change (e.g., `::spec @event-bus — a centralized pub/sub messaging layer` or `::spec @event-bus — channels should support priority ordering`).

Spec is the single command for all pyramid authorship — whether introducing a concept that doesn't exist yet or updating one as requirements and understanding change.

## Procedure

### Step 0: Determine Intent and Resolve P

Determine whether this is a **creation** (new pyramid) or a **revision** (existing pyramid):

1. If P is given and resolves to an existing pyramid file → this is a **revision**. Proceed to the Revision path.
2. If P is given and does not resolve to an existing file → this is a **creation** with a suggested name. Proceed to the Creation path.
3. If P is omitted:
   - Read the user's description to understand what area of the system they're describing
   - Search `./pyramids/` for pyramids that might already cover this area
   - If a match exists, present it and ask the user whether this is a **revision** to that pyramid or a **new** concept
   - If no match exists, this is a **creation**. Proceed to the Creation path.

Do not proceed until intent and target are resolved.

---

## Creation Path

### Step 1: Gather Information

Determine from the user's description:
1. **What the concept is** — its purpose and scope
2. **Where it belongs in the hierarchy** — which parent pyramid owns it
3. **What it relates to** — siblings, dependencies, existing concepts
4. **Whether placeholders exist** — search the codebase for `PYRS_TODO` references to this concept

If placement is unclear from the description, **ask the user** probing questions. Do not guess.

### Step 2: Assess Fit

Read the draft pyramid's **parent pyramid(s)** to assess how the new concept fits conceptually:
- Does it belong under this parent, or would it fit better elsewhere?
- Does it overlap with existing siblings?
- Does it conflict with the parent's constraints?

If the concept doesn't fit cleanly, ask a **second round of probing questions** before proceeding. Getting hierarchy placement wrong will cause audit failures later.

### Step 3: Write the Pyramid

Create the pyramid file with all required sections:

- **Purpose** — what the concept is and why it exists
- **Concepts** — key ideas and behaviors
- **Contracts** — behavioral guarantees and invariants
- **Relationships** — explicit link to parent; links to children if any are known; **See Also** cross-references if the user specifies them (`@` references like `@event-bus.actions` or `@./pyramids/event-bus/actions/index.md` resolve to pyramid paths)
- **Constraints** — boundaries and prohibitions

Place the file at:
- `./pyramids/index.md` for root
- `./pyramids/[slug]/index.md` for non-root concepts

### Step 4: Update the Parent

Add a reference to the new pyramid in the parent's Relationships section. This is **mandatory** — a pyramid without a parent link is an orphan and will fail audits.

### Step 5: Check for See Also Cross-References

Read sibling pyramids (other children of the same parent) and check whether any are conceptually related to the new pyramid. If so, suggest adding See Also links in both directions — on the new pyramid and on the existing sibling. Ask the user before modifying existing pyramids.

### Step 6: Update Code Placeholders

If `PYRS_TODO` placeholders referencing this concept exist in the codebase, note them for the user. The placeholders should remain until `::apply` is run for this pyramid, but the user should know they exist.

---

## Revision Path

### Step 1: Read the Current State

1. **Resolve P** — resolve the `@`-prefixed pyramid reference to its pyramid file (see Pyramid Identifiers in _foundation.md)
2. **Read P thoroughly** — understand its current Purpose, Concepts, Contracts, Relationships, and Constraints
3. **Read parent pyramid(s)** — understand how P fits in the hierarchy today

### Step 2: Understand the Change

From the user's description, determine:
1. **What is changing** — new concepts, revised contracts, relaxed or tightened constraints, shifted purpose
2. **What the impact is** — does this change affect parent alignment? Does it affect children?
3. **Whether the change fits** — does the updated P still belong under its current parent?

If the change is ambiguous or could ripple in multiple directions, **ask probing questions** before proceeding.

### Step 3: Assess Hierarchy Impact

Check whether the update causes drift with parents or children:
- Does the updated purpose still align with how the parent describes P?
- Do updated contracts conflict with parent or sibling contracts?
- Do children of P depend on contracts or concepts being removed or changed?
- Should any children be updated, removed, or created as a result?

If there is hierarchy impact, **surface it to the user** and ask how they want to handle it. Do not silently update multiple pyramids.

### Step 4: Apply the Update

Update the pyramid file, preserving all required sections:
- **Purpose** — revise if the concept's intent has shifted
- **Concepts** — add, remove, or modify as described
- **Contracts** — update behavioral guarantees; flag any that are being removed or weakened
- **Relationships** — update if children are being added, removed, or restructured. Add, update, or remove **See Also** cross-references as needed (`@` references like `@event-bus.actions` or `@./pyramids/event-bus/actions/index.md` resolve to pyramid paths)
- **Constraints** — update boundaries as needed

### Step 5: Flag Downstream Effects

After updating P, inform the user of any downstream actions that may be needed:
- **Children** that may need their own `::spec` to stay aligned
- **Code** that may need `::apply` to conform to the revised pyramid
- **Parent** that may need revision if P's role in the hierarchy shifted

Do not perform these downstream actions automatically — surface them and let the user decide.

### Step 6: Post-Spec Diff Refresh

After a successful spec mutation for target `P`, check whether a sibling `diff.md` exists for `P` either:
- before the mutation started, or
- after the mutation completed.

If yes, run a same-target `::diff P` refresh before final output.

- Keep this refresh strictly target-scoped.
- Do **NOT** broaden into a multi-target diff sweep unless the user explicitly requested that scope.

### Step 7: Post-Spec Handoff Notice

After any successful spec change:
- Tell the user that starting a new session and running `::apply @target` is the recommended path to ensure the updated pyramid produces correct code
- Explicitly declare: `I will NOT initiate any code changes in this session.`
- If the user explicitly issues `::apply` in the same session anyway, allow that user-initiated action and proceed via the apply workflow
