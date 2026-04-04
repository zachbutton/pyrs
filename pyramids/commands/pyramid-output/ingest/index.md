# Ingest

## Purpose

Produce pyramids from existing code. Ingest surveys a codebase (or a targeted area), proposes a concept hierarchy, and writes pyramids top-down after user confirmation at each level.

## Concepts

- **Syntax:** `::ingest P? [...description]` — P optionally targets a specific area of the codebase. If omitted, the agent surveys the entire project.
- **Abstraction from code to concepts.** Ingest reads implementation and abstracts upward to produce conceptual pyramids. An ingest pyramid should be indistinguishable from one created via ::spec — it captures what and why, never how.
- **Top-down writing with user confirmation.** The agent proposes the hierarchy first, then writes pyramids from the root downward, confirming with the user at each level before proceeding.
- **Read-only with respect to code.** Ingest never modifies source code. It only reads code and produces pyramids.
- **Re-ingesting existing pyramids.** When targeted at a pyramid that already exists, ingest reads the current code and proposes updates to the pyramid to reflect the current implementation state. Conflicts between the existing pyramid and the code are surfaced for the user to resolve.
- **Post-ingest handoff notice.** After ingest creates or revises pyramids, the agent notifies the user that starting a new session and running `::apply @target` is the recommended path for reliable code alignment, explicitly declares it will not initiate code changes, and allows a user-initiated `::apply` if the user explicitly chooses to run it in the current session.
- **Post-ingest diff refresh.** If a touched pyramid has a sibling `diff.md`, ingest runs a same-target diff pass after writing pyramids so unresolved findings stay current or the sidecar is removed when resolved.

## Contracts

- Ingest never modifies code — it only reads code and produces pyramids.
- Ingest pyramids are conceptual, not code descriptions. They capture what and why, never how.
- Ingest proposes the hierarchy and requires explicit user confirmation before writing any pyramids.
- New pyramids are always written with all five required sections (Purpose, Concepts, Contracts, Relationships, Constraints).
- Ingest must not invent contracts the code does not currently uphold. It captures what is, not what should be.
- When re-ingesting an existing pyramid, ingest compares the current code against the existing pyramid and proposes updates. Conflicts are surfaced to the user, not silently resolved.
- After ingest changes pyramids, the agent must emit an explicit handoff notice recommending a fresh session and `::apply @target` to ensure the revised or newly created pyramids produce correct code.
- After that notice, the agent explicitly declares it will not initiate code changes on its own.
- If the user explicitly issues `::apply` in the same session, that user-initiated action is allowed.
- After ingest mutates target P, if sibling `diff.md` exists for P before or after the mutation, ingest runs a same-target diff pass before final output.

## Relationships

- Parent: [Pyramid Output](../index.md)
- See Also: [Scan](../../verification/scan/index.md) — after ingest, scan can check structural completeness of the new hierarchy

## Constraints

- Ingest must not modify code under any circumstances.
- Ingest must not invent behavior or contracts that the code does not exhibit. It describes reality, not aspirations.
- After writing or revising pyramids, ingest must not initiate code changes in that session; only a user-explicit `::apply` command may initiate code work.
- Post-ingest diff refresh must remain target-scoped. Ingest must not trigger broad diff sweeps unless the user explicitly requested that scope.
