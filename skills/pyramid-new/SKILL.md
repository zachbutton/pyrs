---
name: pyramid-new
description: "Create a new pyramid from a description or reference. Triggered by ::new description:: where the description explains the new concept."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid New

This skill is activated when the user issues `::new description::` where the description explains a new concept to be documented as a pyramid (e.g., `::new event-bus — a centralized pub/sub messaging layer for decoupled component communication::`).

## Procedure

### Step 1: Gather Information

Determine from the user's description:
1. **What the concept is** — its purpose and scope
2. **Where it belongs in the hierarchy** — which parent pyramid owns it
3. **What it relates to** — siblings, dependencies, existing concepts
4. **Whether placeholders exist** — search the codebase for `PRYS_TODO` references to this concept

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
- **Relationships** — explicit link to parent; links to children if any are known; **See Also** cross-references if the user specifies them (dot-delimited identifiers like `event-bus.actions` resolve to pyramid paths)
- **Constraints** — boundaries and prohibitions

Place the file at:
- `./pyramids/[slug].md` for a small, self-contained concept
- `./pyramids/[slug]/index.md` if the concept will have children

### Step 4: Update the Parent

Add a reference to the new pyramid in the parent's Relationships section. This is **mandatory** — a pyramid without a parent link is an orphan and will fail audits.

### Step 5: Update Code Placeholders

If `PRYS_TODO` placeholders referencing this concept exist in the codebase, note them for the user. The placeholders should remain until `::implement::` is run for this pyramid, but the user should know they exist.
