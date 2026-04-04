# Code Output

## Purpose

Commands that produce or update code from pyramids. This is the only route from concept to code in the system, using incremental test-driven development to ensure code matches contracts.

## Concepts

- **Apply** is the single command that produces and modifies code. It handles both building a pyramid's concept from scratch and updating existing code to conform to a revised pyramid.
- **Test-driven development.** Apply follows strict TDD: write a failing test for a contract, make it pass, move to the next contract. This is non-negotiable and step-by-step — the agent does not batch multiple contracts into one implementation pass.
- **Strict scoping.** The agent implements only what the target pyramid describes. Not the parent's behavior. Not the children's behavior. Not helpful extras. Scope discipline is what makes the pyramid system trustworthy.
- **Placeholders for unbuilt dependencies.** When a child or See Also reference hasn't been implemented, the agent leaves a `// PYRS_TODO: ./pyramids/[path]` placeholder with runtime logging rather than guessing at the dependency's behavior.
- **Post-apply diff refresh.** When the target pyramid already has a sibling `diff.md`, apply runs a same-target diff pass after code changes so the report is immediately updated or removed.

## Contracts

- Apply is the only command that produces or modifies code.
- Apply uses incremental TDD — one contract at a time, red then green, no batching.
- Implementation scope is strictly bounded by the target pyramid. No parent scope, no child scope, no undocumented behavior.
- Unbuilt dependencies produce PYRS_TODO placeholders with meaningful runtime logging, unless the pyramid opts out of code markers.
- Generated code carries PYRS provenance comments linking back to the source pyramid, unless opted out.
- After apply mutates code for target P, if sibling `diff.md` exists for P before or after the mutation, apply runs a same-target diff pass before final output.

## Relationships

- Parent: [Commands](../index.md)
- Children:
  - [Apply](./apply/index.md) — produce or update code from pyramids
- See Also: [Change Flow](../../change-flow/index.md) — apply is the code-producing step of the change flow

## Constraints

- Code output commands must not modify pyramids. They consume pyramids and produce code.
- The agent must not skip TDD or batch multiple contracts. The incremental discipline is what ensures each contract is independently verified.
- Placeholders must not be replaced with guesses. Only a subsequent apply of the dependency's pyramid replaces a placeholder.
- Post-apply diff refresh must stay target-scoped. Code output commands must not run broad multi-target diff sweeps unless the user explicitly requested that scope.
