# Commands

## Purpose

The command vocabulary through which users interact with the pyramid workflow. Commands are issued in conversation using `::command context::` syntax. Each command loads a specialized strategy; the agent handles the rest. Commands are the interface between human intent and the pyramid system.

## Concepts

- **Double-colon syntax.** Commands use `::command context::` — two colons on each side. The first token is the command; everything after is context, arguments, or description.
- **Pyramid identifiers as arguments.** Most commands accept a dot-delimited pyramid identifier (P) that targets a specific pyramid. Some commands accept optional identifiers with fallback behavior.
- **Natural language context.** Beyond the identifier, command context is conversational. Users describe what they want in plain English.
- **Change interception.** When a user describes a code change without using a command, the system redirects them into the pyramid workflow — identifying which pyramid to create or update first.
- **Command categories.** Commands group into five areas: creation (producing pyramids), verification (checking alignment), implementation (producing code), maintenance (evolving the system over time), and utility (read-only informational commands).

## Contracts

- Every command operates through the pyramid system — no command directly modifies code. Commands either produce/modify pyramids or produce/modify code from pyramids, never both in one operation. Bootstrap is read-only with respect to code — it only produces pyramids.
- Commands that modify pyramids or code always require user confirmation before making changes.
- The `::command::` syntax is the only entry point for pyramid operations. Plain-language change requests are intercepted and routed to the appropriate command.
- Each command has a defined scope and does not silently perform the work of other commands.

## Relationships

- Parent: [Pyrs](../index.md)
- Children:
  - [Creation](./creation.md) — commands that produce new pyramids
  - [Verification](./verification.md) — commands that check alignment and completeness
  - [Implementation](./implementation.md) — commands that produce or update code from pyramids
  - [Maintenance](./maintenance.md) — commands that evolve pyramids and diagnose issues
  - [Utility](./utility.md) — read-only commands for navigating and understanding the pyramid system
- See Also: [Change Flow](../change-flow.md) — commands are the mechanism through which the change flow is executed

## Constraints

- Commands must not be combinable or chainable in a single invocation. Each command is one operation with one clear outcome.
- No command may silently invoke another command. If a command discovers work that belongs to a different command, it surfaces this to the user.
- The command vocabulary must not grow to include code-level operations (refactor, format, lint). Commands operate at the concept level.
