# Apply

## Purpose

Produce or update code from a pyramid using incremental test-driven development. Apply is the only route from concept to code — it handles both building from scratch and updating existing code to match a revised pyramid.

## Concepts

- **Syntax:** `::apply P` — P is an `@`-prefixed pyramid reference targeting the pyramid to implement or re-align code to. P may be identifier form (for example, `@auth.oauth` or `@root.auth.oauth`) or a direct pyramid `index.md` link (for example, `@./pyramids/auth/oauth/index.md`).
- **Building from scratch.** When no code exists for the pyramid, the agent reads the pyramid, detects the project's test framework, and works through red/green cycles incrementally — one contract at a time, never in batch.
- **Updating to match revisions.** When code already exists and the pyramid has been revised via ::spec, the agent compares the updated pyramid against existing code, identifies gaps, and tightens incrementally. Code the pyramid no longer describes is removed.
- **Strict TDD.** Write a failing test for a contract, make it pass, move to the next contract. Non-negotiable and step-by-step.
- **Strict scoping.** Only what the target pyramid describes. Not the parent's behavior, not the children's behavior, not helpful extras.
- **PYRS_TODO placeholders.** Unbuilt dependencies get `// PYRS_TODO: ./pyramids/[path]` placeholders with runtime logging rather than guessed implementations.
- **PYRS provenance comments.** Generated code carries comments linking back to the source pyramid, unless the pyramid opts out.
- **Post-apply diff reconciliation.** If the target pyramid has a sibling `diff.md`, apply runs a same-target diff pass after code updates so the report is either refreshed with remaining gaps or deleted when fully reconciled.

## Contracts

- Apply is the only command that produces or modifies code.
- Apply uses incremental TDD — one contract at a time, red then green, no batching.
- Implementation scope is strictly bounded by the target pyramid.
- Unbuilt dependencies produce PYRS_TODO placeholders with meaningful runtime logging, unless the pyramid opts out of code markers.
- Generated code carries PYRS provenance comments, unless opted out.
- When updating, code the pyramid no longer describes is removed.
- After apply mutates code for target P, if sibling `diff.md` exists for P before or after the mutation, apply must run a same-target diff pass before final output.

## Relationships

- Parent: [Code Output](../index.md)
- See Also: [Spec](../../pyramid-output/spec/index.md) — spec revises pyramids, apply updates code to match
- See Also: [Mend](../../pyramid-output/mend/index.md) — mend patches pyramids, then routes to apply for the code fix

## Constraints

- Apply must not modify pyramids. It consumes pyramids and produces code.
- The agent must not skip TDD or batch multiple contracts.
- Placeholders must not be replaced with guesses. Only a subsequent apply of the dependency's pyramid replaces a placeholder.
- Post-apply diff refresh must be limited to the target scope. Apply must not trigger broad diff sweeps unless the user explicitly requested that scope.
