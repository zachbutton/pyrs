# Maintenance

## Purpose

Commands that evolve the pyramid system over time: revising pyramids as understanding changes, diagnosing failures that the pyramids should have prevented, and intercepting unstructured change requests to route them through the workflow.

## Concepts

- **Update (`::update P? [...description]::`)** — revises an existing pyramid when requirements or understanding changes. If P is omitted, the agent asks the user which pyramid they mean, suggesting candidates based on the description. The agent reads the current state and parent, understands what's changing, assesses hierarchy impact, applies the update, and flags downstream effects. It surfaces impacts to related pyramids rather than silently propagating changes.
- **Post-mortem (`::post-mortem P? [...description]::`)** — investigates an issue that should have been impossible given the pyramids. If P is omitted, the agent asks the user which pyramid governs the affected area, suggesting candidates based on the description. The agent debugs to find the root cause, then traces it back to a pyramid gap: a missing contract, an under-specified constraint, a missing concept, or a cross-pyramid interaction nobody accounted for. The fix is always a pyramid patch — the code fix follows via tighten.
- **Change interception** — when a user describes a code change without using a `::` command, the system identifies the governing pyramid and guides the user to the appropriate command (new, update, or implement). It never modifies code directly.

## Contracts

- Update modifies only the target pyramid and its parent's child reference if needed. It does not silently update other pyramids.
- Update surfaces downstream impacts (affected children, See Also references) to the user rather than auto-propagating changes.
- Post-mortem always traces the root cause back to a pyramid gap. The bug is a symptom; the real problem is in the pyramids.
- Post-mortem proposes pyramid patches but does not modify code directly. Code fixes follow via tighten.
- Change interception never modifies code or pyramids. It only identifies the right command and asks the user to confirm.

## Relationships

- Parent: [Commands](./index.md)
- See Also: [Creation](./creation.md) — update evolves existing pyramids; creation produces new ones
- See Also: [Implementation](./implementation.md) — post-mortem patches pyramids, then tighten updates code

## Constraints

- Update must not silently cascade changes through the hierarchy. Each affected pyramid must be surfaced and handled with user confirmation.
- Post-mortem must not fix code directly. The pyramid gap must be patched first; the code fix is always a downstream consequence.
- Change interception must not bypass the pyramid workflow under any circumstances, even if the user's request seems simple.
