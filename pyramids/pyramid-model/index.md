# Pyramid Model

## Purpose

Defines what a pyramid is: its structure, how it's identified, where it lives in the filesystem, and the rules governing its content. This is the data model that all commands operate on.

## Concepts

- **File layout.** Pyramids live in a `./pyramids/` directory. A concept with children gets a directory with an `index.md`. A small, self-contained concept gets a single `.md` file. Nesting is recursive.
- **Required sections.** Every pyramid file contains Purpose, Concepts, Contracts, Relationships, and Constraints. These sections are not optional — missing sections are audit failures.
- **Dot-delimited identifiers.** Pyramids are referenced using dot notation (e.g., `event-bus.actions`) that resolves to file paths. `root` refers to `./pyramids/index.md` and is optional as a prefix.
- **Content is conceptual.** Pyramids describe what and why, never how. Code snippets are permitted only as abstract illustrations — they are never prescriptive. Real code must match the concept, not the example.
- **Parent-child linking.** Every pyramid must reference its parent, and every parent must reference its children. These bidirectional links are mandatory and enforced by audits.
- **See Also references.** Cross-references to related pyramids that are not in a parent-child relationship. These use dot-delimited identifiers and create non-hierarchical connections across the tree.

## Contracts

- Every pyramid file contains all five required sections: Purpose, Concepts, Contracts, Relationships, Constraints.
- Dot-delimited identifiers resolve unambiguously to exactly one file path (`slug.md` or `slug/index.md`).
- Parent-child links are always bidirectional — a child references its parent and the parent references the child.
- A pyramid with no parent link (other than root) is an orphan and fails audits.
- Pyramid content never prescribes implementation. Code examples are illustrative only.

## Relationships

- Parent: [Pyrs](../index.md)
- See Also: [Change Flow](../change-flow.md) — the change flow depends on the model's structure and rules

## Constraints

- The pyramid model must not expand to include runtime or executable semantics. Pyramids are inert markdown.
- Identifiers must remain simple dot-delimited slugs — no versioning, tagging, or metadata encoding in identifiers.
- The five required sections must not be extended with additional mandatory sections without updating all commands that read and write pyramids.
