# Help

## Purpose

Display the full command reference showing all available pyramid commands, their syntax, and argument conventions.

## Concepts

- **Syntax:** `::help` — takes no arguments.
- **Static reference.** The output is a complete listing of commands with signatures and brief descriptions. It is not context-sensitive.

## Contracts

- Help output accurately reflects current command signatures, including which arguments are required vs. optional.
- Help lists `::` standalone as a way to make the agent aware of PYRS rules without performing an operation.
- Help does not produce side effects of any kind.

## Relationships

- Parent: [Utility](../index.md)

## Constraints

- Help must not modify pyramids or code.
- Help must not offer guidance or recommendations — it is a reference, not an advisor.
