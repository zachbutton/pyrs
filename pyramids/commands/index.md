# Commands

## Purpose

The command vocabulary through which users interact with the pyramid workflow. Commands are issued in conversation using `::command context` syntax. Each command loads a specialized strategy; the agent handles the rest. Commands are the interface between human intent and the pyramid system.

## Concepts

- **Double-colon prefix syntax.** Commands use `::command context` — two colons only at the start. The first token is the command; everything after is context, arguments, or description.
- **`@` pyramid references as arguments.** Most commands accept an `@`-prefixed pyramid reference (P) that targets a specific pyramid. References may use dot notation (for example, `@root.auth.oauth` or `@auth.oauth`) or a direct path to a pyramid `index.md` file (for example, `@./pyramids/auth/oauth/index.md`). Some commands accept optional references with fallback behavior.
- **Natural language context.** Beyond the identifier, command context is conversational. Users describe what they want in plain English.
- **Change interception.** When a user describes a code change without using a command, the system redirects them into the pyramid workflow — identifying which pyramid to create or update first.
- **Standalone `::` signal.** `::` can be invoked alone, without a command name. This invokes the foundation skill, which makes the agent aware of the PYRS workflow rules. It is not a command — it performs no operation and modifies no state. It is a nudge for situations where the agent should understand the project's PYRS architecture even though the user isn't performing a pyramid operation.
- **Command categories.** Commands group into four areas: pyramid-output (producing pyramids and tracing issues to pyramid gaps), code-output (producing code), verification (checking alignment), and utility (read-only informational commands).
- **Post-mutation diff refresh.** Commands that modify pyramids or code include an explicit post-mutation verification phase for the same target when a sibling `diff.md` is present, so unresolved reports are refreshed or removed immediately.

## Contracts

- Every command operates through the pyramid system — no command directly modifies code. Commands either produce/modify pyramids or produce/modify code from pyramids, never both in one operation. Ingest is read-only with respect to code — it only produces pyramids.
- Commands that modify pyramids or code always require user confirmation before making changes.
- The `::command` syntax is the only entry point for pyramid operations. Plain-language change requests are intercepted and routed to the appropriate command.
- Commands that accept P resolve `@` references consistently whether provided as dot notation or direct pyramid `index.md` links.
- Each command has a defined scope and does not silently perform the work of other commands.
- After a command modifies pyramids or code for target P, if a sibling `diff.md` exists for that target before or after the mutation, the command runs a same-target diff pass before returning results.

## Relationships

- Parent: [Pyrs](../index.md)
- Children:
  - [Pyramid Output](./pyramid-output/index.md) — commands that produce new pyramids and trace issues to pyramid gaps (spec, ingest, mend)
  - [Code Output](./code-output/index.md) — commands that produce or update code from pyramids (apply)
  - [Verification](./verification/index.md) — commands that check alignment and completeness (sane, scan, diff)
  - [Utility](./utility/index.md) — read-only commands for navigating and understanding the pyramid system (ls, help)
- See Also: [Change Flow](../change-flow/index.md) — commands are the mechanism through which the change flow is executed

## Constraints

- Commands must not be combinable or chainable in a single invocation. Each command is one operation with one clear outcome.
- No command may silently invoke another command. The sole built-in exception is the explicit same-target post-mutation diff refresh for touched targets with `diff.md` sidecars.
- Bare dot identifiers without `@` must not be treated as the canonical command argument form.
- The command vocabulary must not grow to include code-level operations (refactor, format, lint). Commands operate at the concept level.
