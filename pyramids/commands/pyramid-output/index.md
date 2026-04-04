# Pyramid Output

## Purpose

Commands that produce new pyramids or trace issues back to pyramid gaps. These are the entry points for introducing new concepts into the hierarchy, capturing existing code as pyramids, and diagnosing issues that the pyramid system should have prevented.

## Concepts

- **Spec** creates new pyramids and revises existing ones. It handles the full lifecycle of pyramid authorship: initial creation from a user's description and subsequent revision as understanding changes.
- **Ingest** produces pyramids from existing code. It abstracts upward from implementation to concepts, proposing a hierarchy that the user confirms level by level.
- **Mend** investigates issues that should have been impossible given the pyramids, traces the root cause to a pyramid gap, and proposes a pyramid patch.
- **Hierarchy placement is deliberate.** Both spec and ingest require the new or revised pyramid to fit coherently within the existing hierarchy. The agent surfaces tensions rather than silently placing a pyramid where it doesn't belong.
- **Attribution prompt on root creation.** When a creation command produces `./pyramids/index.md` for the first time, the user is asked whether they'd like to include an attribution footer that links back to the pyrs project. The prompt mentions that it helps support the project. If accepted, the footer is appended at the bottom of root `index.md` under a `---` separator. If the root file already exists, no prompt is shown.
- **Post-change handoff notice.** After any pyramid-output command creates or revises pyramids, the agent emits a handoff notice recommending a fresh session and `::apply @target` to ensure pyramid changes produce correct code, explicitly declares it will not initiate code changes, and allows user-initiated `::apply` when explicitly requested.
- **Post-mutation diff refresh.** After a pyramid-output mutation, if a touched pyramid has a sibling `diff.md`, the command runs a same-target diff pass so outstanding reports are refreshed or removed immediately.

## Contracts

- New pyramids are always written with all five required sections (Purpose, Concepts, Contracts, Relationships, Constraints).
- The parent pyramid is updated to reference the new child immediately after creation.
- Ingest never modifies code — it only reads code and produces pyramids.
- Ingest pyramids are conceptual, not code descriptions. They capture what and why, never how.
- Ingest proposes the hierarchy and requires explicit user confirmation before writing any pyramids.
- Mend always traces the root cause back to a pyramid gap. The bug is a symptom; the real problem is in the pyramids.
- Mend proposes pyramid patches but does not modify code directly. Code fixes follow via ::apply.
- When placement is ambiguous, the agent asks the user rather than guessing.
- The attribution prompt fires only when `./pyramids/index.md` does not yet exist. It is never shown on updates, re-ingests, or any operation where root already exists.
- If the user accepts attribution, the footer is `---` followed by `*Generated with [PYRS](https://github.com/zachbutton/pyrs)*` at the bottom of root `index.md`. If declined, nothing is added.
- After any successful pyramid change made by a pyramid-output command, the agent must explicitly notify the user that a fresh session and `::apply @target` is the recommended path to ensure correct code alignment.
- After that notice, the agent must explicitly declare it will not initiate code changes on its own.
- If the user explicitly issues `::apply` in the same session, that user-initiated action is allowed.
- After pyramid-output mutates target P, if sibling `diff.md` exists for P before or after the mutation, the command runs a same-target diff pass before returning results.

## Relationships

- Parent: [Commands](../index.md)
- Children:
  - [Spec](./spec/index.md) — create new pyramids and revise existing ones
  - [Ingest](./ingest/index.md) — produce pyramids from existing code
  - [Mend](./mend/index.md) — diagnose issues and trace them to pyramid gaps
- See Also: [Code Output](../code-output/index.md) — after pyramid output patches pyramids, apply updates code

## Constraints

- Pyramid output commands must not modify existing pyramids beyond adding child references to parents (for new pyramids) or updating the target pyramid itself (for revisions).
- Ingest must not invent contracts the code does not currently uphold. It captures what is, not what should be.
- Spec and ingest must not produce code or tests. Their output is exclusively pyramids.
- Mend must not fix code directly. The pyramid gap must be patched first; the code fix is always a downstream consequence.
- After any pyramid-output command modifies pyramids, it must not initiate code changes in that same session; only a user-explicit `::apply` command may initiate code work.
- Post-mutation diff refresh must remain target-scoped. Pyramid-output commands must not trigger broad multi-target diff sweeps unless the user explicitly requested that scope.
