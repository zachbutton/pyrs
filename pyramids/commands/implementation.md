# Implementation

## Purpose

Commands that produce or update code from pyramids. These are the only routes from concept to code in the system. Both use incremental test-driven development to ensure code matches contracts.

## Concepts

- **Implement (`::implement P::`)** — builds P's concept in code from scratch using TDD. The agent reads the pyramid, detects the project's test framework, and works through red/green cycles incrementally — one contract at a time, never in batch. Unbuilt dependencies get PYRS_TODO placeholders. Code carries PYRS provenance comments.
- **Tighten (`::tighten P::`)** — updates existing code to conform to a revised pyramid, also using TDD. The agent compares the updated pyramid against existing code, identifies gaps, and tightens incrementally. Code the pyramid no longer describes is removed.
- **Test-driven development.** Both commands follow strict TDD: write a failing test for a contract, make it pass, move to the next contract. This is non-negotiable and step-by-step — the agent does not batch multiple contracts into one implementation pass.
- **Strict scoping.** The agent implements only what the target pyramid describes. Not the parent's behavior. Not the children's behavior. Not helpful extras. Scope discipline is what makes the pyramid system trustworthy.
- **Placeholders for unbuilt dependencies.** When a child or See Also reference hasn't been implemented, the agent leaves a `// PYRS_TODO: ./pyramids/[path]` placeholder with runtime logging rather than guessing at the dependency's behavior.

## Contracts

- Implement and tighten are the only commands that produce or modify code.
- Both commands use incremental TDD — one contract at a time, red then green, no batching.
- Implementation scope is strictly bounded by the target pyramid. No parent scope, no child scope, no undocumented behavior.
- Unbuilt dependencies produce PYRS_TODO placeholders with meaningful runtime logging, unless the pyramid opts out of code markers.
- Generated code carries PYRS provenance comments linking back to the source pyramid, unless opted out.
- Tighten removes code that the revised pyramid no longer describes.

## Relationships

- Parent: [Commands](./index.md)
- See Also: [Change Flow](../change-flow.md) — implement and tighten are the code-producing steps of the change flow

## Constraints

- Implementation commands must not modify pyramids. They consume pyramids and produce code.
- The agent must not skip TDD or batch multiple contracts. The incremental discipline is what ensures each contract is independently verified.
- Placeholders must not be replaced with guesses. Only a subsequent implement of the dependency's pyramid replaces a placeholder.
