# Utility

## Purpose

Read-only commands that help users navigate and understand the pyramid system without modifying anything. These are informational tools — they show structure and available operations, never change state.

## Concepts

- **List (`::ls::` / `::ls describe::` / `::ls d::`)** — displays the pyramid hierarchy as a tree using dot-delimited identifiers. With `describe` or `d`, appends a one-sentence purpose from each pyramid file. Structure-only mode does not read file contents.
- **Help (`::help::`)** — displays the full command reference showing all available pyramid commands, their syntax, and argument conventions. The output is a static reference, not context-sensitive.
- **Read-only.** Neither command modifies pyramids, code, or any project state. They are safe to run at any time.

## Contracts

- `ls` output reflects the current filesystem state of `./pyramids/` — it does not cache or assume structure.
- `ls describe` reads each pyramid file to extract its purpose. Descriptions are kept to one sentence.
- `help` output accurately reflects current command signatures, including which arguments are required vs. optional.
- Neither command produces side effects of any kind.

## Relationships

- Parent: [Commands](./index.md)

## Constraints

- Utility commands must not modify pyramids or code under any circumstances.
- Utility commands must not interpret or analyze content beyond what is needed for display (e.g., `ls` shows structure, not alignment assessments).
- Help must not offer guidance or recommendations — it is a reference, not an advisor.
