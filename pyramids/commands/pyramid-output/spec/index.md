# Spec

## Purpose

Create new pyramids and revise existing ones. This is the single command for all pyramid authorship — whether introducing a concept that doesn't exist yet or updating one as requirements and understanding change. After spec updates a pyramid, it recommends a fresh-session apply handoff and declares that the agent will not initiate code changes.

## Concepts

- **Syntax:** `::spec P? [...description]` — P is an `@`-prefixed pyramid reference, either identifier form (for example, `@auth.oauth` or `@root.auth.oauth`) or a direct pyramid `index.md` link (for example, `@./pyramids/auth/oauth/index.md`). If omitted during creation, the agent works with the user to determine placement. If omitted during revision, the agent asks which pyramid the user means, suggesting candidates based on the description.
- **New pyramid creation.** The agent asks probing questions about where the pyramid fits in the hierarchy, writes it with all five required sections, and immediately updates the parent to reference the new child.
- **Pyramid revision.** The agent reads the current pyramid and its parent, understands what's changing, assesses hierarchy impact, applies the update, and flags downstream effects. Impacts to related pyramids are surfaced rather than silently propagated.
- **Hierarchy placement is deliberate.** The agent surfaces tensions in placement rather than silently placing a pyramid where it doesn't belong.
- **Post-spec handoff notice.** After any successful spec change, the agent notifies the user that starting a new session and running `::apply @target` is the recommended path for reliable code alignment, explicitly declares it will not initiate code changes, and allows a user-initiated `::apply` if the user explicitly chooses to run it in the current session.
- **Post-spec diff refresh.** If the touched pyramid has a sibling `diff.md`, spec runs a same-target diff pass after the pyramid update so unresolved findings stay current or the sidecar is removed when resolved.

## Contracts

- New pyramids are always written with all five required sections (Purpose, Concepts, Contracts, Relationships, Constraints).
- The parent pyramid is updated to reference the new child immediately after creation.
- Revisions modify only the target pyramid and its parent's child reference if needed. Other pyramids are not silently updated.
- Downstream impacts (affected children, See Also references) are surfaced to the user rather than auto-propagated.
- When placement is ambiguous, the agent asks the user rather than guessing.
- After any successful spec update, the agent emits an explicit handoff notice recommending a fresh session and `::apply @target` to ensure the revised pyramid produces correct code.
- After that notice, the agent explicitly declares it will not initiate code changes on its own.
- If the user explicitly issues `::apply` in the same session, that user-initiated action is allowed.
- After spec mutates target P, if sibling `diff.md` exists for P before or after the mutation, spec runs a same-target diff pass before final output.

## Relationships

- Parent: [Pyramid Output](../index.md)
- See Also: [Apply](../../code-output/apply/index.md) — after spec revises a pyramid, apply updates code to match
- See Also: [Mend](../mend/index.md) — mend traces issues to pyramid gaps, then routes to spec for the fix

## Constraints

- Spec must not modify code. Its output is exclusively pyramids.
- Spec must not silently cascade changes through the hierarchy. Each affected pyramid must be surfaced and handled with user confirmation.
- After completing a spec change, spec must not initiate code changes in that session; only a user-explicit `::apply` command may initiate code work.
- Post-spec diff refresh must remain target-scoped. Spec must not trigger broad diff sweeps unless the user explicitly requested that scope.
