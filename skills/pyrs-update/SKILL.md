---
name: pyrs-update
description: "Update an existing pyramid based on new information or changed requirements. Triggered by ::update P [description]:: where P is a pyramid identifier."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid Update

This skill is activated when the user issues `::update P description::` where P is an existing pyramid identifier and the description explains what needs to change (e.g., `::update event-bus — channels should support priority ordering::`).

## Procedure

### Step 1: Read the Current State

1. **Resolve P** — resolve the dot-delimited identifier to its pyramid file (see Pyramid Identifiers in _foundation.md)
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
- **Relationships** — update if children are being added, removed, or restructured. Add, update, or remove **See Also** cross-references as needed (dot-delimited identifiers like `event-bus.actions` resolve to pyramid paths)
- **Constraints** — update boundaries as needed

### Step 5: Flag Downstream Effects

After updating P, inform the user of any downstream actions that may be needed:
- **Children** that may need their own `::update::` to stay aligned
- **Code** that may need `::tighten::` to conform to the revised pyramid
- **Parent** that may need revision if P's role in the hierarchy shifted

Do not perform these downstream actions automatically — surface them and let the user decide.
