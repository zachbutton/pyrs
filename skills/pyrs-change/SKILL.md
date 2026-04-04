---
name: pyrs-change
description: "Invoke pyrs-foundation first. Intercepts when a user describes a code change, feature, or bug fix without using a :: command. Guides them into the pyramid workflow instead of modifying code directly."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules — especially the Change Flow section.

# Pyramid Change Interceptor

This skill activates when a user describes a change, feature request, bug fix, or modification to the project **without** using a `::` pyramid command. For example:

- "Add retry logic to the event bus"
- "The auth flow should support MFA"
- "Fix the race condition in session management"

## What to Do

**Do NOT modify code directly.** All changes flow through the pyramids.

### Step 1: Identify the Pyramid

Determine which pyramid governs the area the user is describing:
- Search `./pyramids/` for the relevant concept
- If multiple pyramids could apply, narrow it down

If no matching pyramid exists, tell the user and suggest `::spec`.

### Step 2: Confirm with the User

Present your finding:
- Which pyramid you identified as the right place for this change
- Whether this is a new concept or a revision to an existing one — either way, `::spec` handles both
- Ask the user to confirm before proceeding

Do **NOT** skip confirmation. Do **NOT** modify any pyramid or code without the user's agreement.

### Step 3: Guide to the Right Command

Once confirmed, guide the user to the appropriate command:

- **New or revised pyramid** → `::spec P? [description]` where `P` is an optional `@`-prefixed pyramid reference
- **Then apply to code** → `::apply P` after the pyramid is written/updated, using an `@`-prefixed pyramid reference

If the target already has a sibling `diff.md`, the mutation command should run a same-target diff refresh before final output so unresolved-gap sidecars stay current.

Remind the user that `::apply` is the only route to code changes.
