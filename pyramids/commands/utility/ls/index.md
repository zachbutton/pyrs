# Ls

## Purpose

Display the pyramid hierarchy as a tree using `@`-prefixed pyramid references, optionally with one-sentence descriptions and unresolved-drift markers.

## Concepts

- **Syntax:** `::ls` / `::ls describe` / `::ls d` — with no arguments, shows the hierarchy tree. With `describe` or `d`, appends a one-sentence purpose from each pyramid `index.md` file.
- **Structure-only mode.** Without the describe flag, ls reflects filesystem structure and sidecar presence under `./pyramids/` without parsing markdown content.
- **Diff callouts in the tree.** When a pyramid has a sibling `diff.md`, ls marks that node directly in the tree so unresolved reconciliation work is visible during navigation.
- **Describe mode.** Reads each pyramid `index.md` file to extract its Purpose section and condenses it to one sentence.

## Contracts

- Ls output reflects the current filesystem state of `./pyramids/`, including whether each pyramid has a sibling `diff.md` that indicates unresolved drift.
- Ls describe reads each pyramid `index.md` file to extract its purpose. Descriptions are kept to one sentence.
- Ls does not produce side effects of any kind.

## Relationships

- Parent: [Utility](../index.md)
- See Also: [Diff](../../verification/diff/index.md) — diff manages actionable `diff.md` sidecars that ls surfaces

## Constraints

- Ls must not modify pyramids or code.
- Ls must not interpret or analyze content beyond what is needed for display. Diff callouts are based on `diff.md` file presence, not report parsing.
