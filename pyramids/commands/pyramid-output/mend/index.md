# Mend

## Purpose

Investigate an issue that should have been impossible given the pyramids and trace it back to a pyramid gap. The fix is always a pyramid patch — the code fix follows via ::apply.

## Concepts

- **Syntax:** `::mend P? [...description]` — P optionally identifies the pyramid governing the affected area. If omitted, the agent asks the user which pyramid is relevant, suggesting candidates based on the description.
- **Root cause tracing.** The agent debugs to find the root cause, then traces it back to the pyramids: a missing contract, an under-specified constraint, a missing concept, or a cross-pyramid interaction nobody accounted for.
- **Pyramid-first fix.** The fix is always a pyramid patch. The bug is a symptom; the real problem is in the pyramids. Code fixes follow via ::apply after the pyramid is corrected.
- **Post-patch handoff notice.** After mend applies confirmed pyramid patches, the agent notifies the user that starting a new session and running `::apply @target` is the recommended path for reliable code alignment, explicitly declares it will not initiate code changes, and allows a user-initiated `::apply` if the user explicitly chooses to run it in the current session.
- **Post-mend diff refresh.** If the touched pyramid has a sibling `diff.md`, mend runs a same-target diff pass after the pyramid patch so unresolved findings stay current or the sidecar is removed when resolved.

## Contracts

- Mend always traces the root cause back to a pyramid gap.
- Mend proposes pyramid patches but does not modify code directly.
- Code fixes follow via ::apply after the pyramid patch.
- The agent surfaces the specific pyramid gap with evidence, not vague assessments.
- After mend-driven pyramid changes, the agent must emit an explicit handoff notice recommending a fresh session and `::apply @target` to ensure the patched pyramids produce correct code.
- After that notice, the agent explicitly declares it will not initiate code changes on its own.
- If the user explicitly issues `::apply` in the same session, that user-initiated action is allowed.
- After mend mutates target P, if sibling `diff.md` exists for P before or after the mutation, mend runs a same-target diff pass before final output.

## Relationships

- Parent: [Pyramid Output](../index.md)
- See Also: [Spec](../spec/index.md) — mend identifies the gap; spec applies the pyramid patch
- See Also: [Apply](../../code-output/apply/index.md) — after the pyramid patch, apply updates code

## Constraints

- Mend must not fix code directly. The pyramid gap must be patched first.
- Mend must not propose code-only fixes that skip the pyramid layer.
- After patching pyramids, mend must not initiate code changes in that session; only a user-explicit `::apply` command may initiate code work.
- Post-mend diff refresh must remain target-scoped. Mend must not trigger broad diff sweeps unless the user explicitly requested that scope.
