---
name: pyrs-mend
description: "Invoke pyrs-foundation first. Investigate an issue that should have been impossible given the pyramids, debug it, and patch the relevant pyramid(s) to prevent recurrence. Triggered by ::mend P? [description] where P optionally targets a specific pyramid reference."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid Mend

This skill is activated when the user issues `::mend P? [...description]` where P is an optional `@`-prefixed pyramid reference and the description explains a bug, failure, or unexpected behavior that *should not have been possible* given the contracts and constraints defined in the pyramids.

If this issue happened, then the pyramids have a gap — a missing contract, an under-specified constraint, an unaccounted-for interaction, or a concept that was never captured.

## Procedure

### Step 0: Resolve P (if omitted)

If the user did not specify a pyramid reference, do NOT guess. Instead:
1. Read the user's description to understand what area of the system is affected
2. Search `./pyramids/` for pyramids that likely govern that area
3. Present the candidate pyramid(s) and ask the user to confirm which one(s) to investigate

Do not proceed until the governing pyramid(s) are identified.

### Step 1: Understand the Issue

Get a clear picture of what went wrong:
- What was the observed behavior?
- What was the expected behavior?
- Under what conditions did it occur?

Ask the user for details if the description is insufficient.

### Step 2: Debug

Investigate the code to find the root cause:
- Trace the failure through the relevant code paths
- Identify the exact mechanism — what code did or didn't do that led to the issue

### Step 3: Find the Pyramid Gap

This is the critical step. The bug is a symptom — the real problem is in the pyramids.

Read the pyramids that govern the affected area and ask:
- **Missing contract** — was there a behavioral guarantee that should have been stated but wasn't?
- **Under-specified constraint** — was a boundary too loose, allowing an interaction that should have been prohibited?
- **Missing concept** — is there an idea or edge case the pyramid never captured?
- **Cross-pyramid gap** — did the issue arise from an interaction between concepts whose pyramids don't account for each other? (missing See Also, missing constraints about how they compose)
- **Scope leak** — did an implementation exceed its pyramid's scope in a way that created the issue?

### Step 4: Propose Pyramid Patches

A single issue may reveal gaps across **multiple pyramids** — for example, a missing contract in one, an under-specified constraint in another, and a missing See Also between two that interact in ways neither accounted for. Propose all necessary changes together so the user can see the full picture.

Present your findings to the user:
- Which pyramid(s) have gaps, and what each one is missing
- Proposed additions or revisions for **each** affected pyramid (new contracts, tightened constraints, new See Also references, new Constraints)
- Whether any entirely new pyramid is needed to capture a concept that was never documented

**Ask the user to confirm** before modifying any pyramid.

### Step 5: Apply

Once confirmed:
- Update the relevant pyramid(s) via `::spec`
- For each patched target pyramid `P`, if sibling `diff.md` exists before or after the mutation, run a same-target `::diff P` refresh before final output
  - Keep refresh strictly target-scoped
  - Do **NOT** run a broad diff sweep unless the user explicitly requested that scope
- After pyramid changes are in place, tell the user that starting a new session and running `::apply @target` is the recommended path to ensure the patched pyramids produce correct code
- Explicitly declare: `I will NOT initiate any code changes in this session.`
- If the user explicitly issues `::apply` in the same session anyway, allow that user-initiated action and proceed via the apply workflow

Do **NOT** fix the code directly. The fix flows through the pyramids.
