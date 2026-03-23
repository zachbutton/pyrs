---
name: pyramid-change
description: "Intercepts when a user describes a code change, feature, or bug fix without using a :: command. Guides them into the pyramid workflow instead of modifying code directly."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules — especially the Change Flow section.

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

If no matching pyramid exists, tell the user and suggest `::new::`.

### Step 2: Confirm with the User

Present your finding:
- Which pyramid you identified as the right place for this change
- Whether this is a new concept (`::new::`) or a revision to an existing one (`::update::`)
- Ask the user to confirm before proceeding

Do **NOT** skip confirmation. Do **NOT** modify any pyramid or code without the user's agreement.

### Step 3: Guide to the Right Command

Once confirmed, guide the user to the appropriate command:

- **New concept** → `::new [description]::`
- **Change to existing pyramid** → `::update P [description]::`
- **Then implement** → `::implement P::` or `::tighten P::` after the pyramid is written/updated

Remind the user that `::implement::` and `::tighten::` are the only routes to code changes.
