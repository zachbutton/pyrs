# Pyramid Model

## Purpose

Defines what a pyramid is: its structure, how it's identified, where it lives in the filesystem, and the rules governing its content. This is the data model that all commands operate on.

## Concepts

- **File layout.** Pyramids live in a `./pyramids/` directory. Root is `./pyramids/index.md`, and every non-root concept uses directory form with an `index.md` (for example, `./pyramids/event-bus/actions/index.md`). Nesting is recursive.
- **Derived diff sidecars.** A sibling `diff.md` can exist beside a pyramid `index.md` only as generated verification output for unresolved pyramid-to-code discrepancies. These sidecars are not pyramid definitions and are deleted when discrepancies are fully reconciled.
- **Required sections.** Every pyramid file contains Purpose, Concepts, Contracts, Relationships, and Constraints. These sections are not optional — missing sections are audit failures.
- **`@` pyramid references.** Pyramids are referenced with an `@` prefix. Identifier form uses dot notation (for example, `@event-bus.actions`), and direct-link form uses a pyramid `index.md` path (for example, `@./pyramids/event-bus/actions/index.md`). `@root` refers to `./pyramids/index.md`, and `root` remains optional inside identifier form (`@root.event-bus.actions` and `@event-bus.actions` target the same pyramid).
- **Content is conceptual.** Pyramids describe what and why, never how. Code snippets are permitted only as abstract illustrations — they are never prescriptive. Real code must match the concept, not the example.
- **Parent-child linking.** Every pyramid must reference its parent, and every parent must reference its children. These bidirectional links are mandatory and enforced by audits.
- **See Also references.** Cross-references to related pyramids that are not in a parent-child relationship. These use `@` pyramid references and create non-hierarchical connections across the tree.

## Contracts

- Every pyramid file contains all five required sections: Purpose, Concepts, Contracts, Relationships, Constraints.
- Identifier-form `@` references resolve unambiguously to exactly one canonical `index.md` path (`slug/index.md` for non-root pyramids, `index.md` for root), and direct-link `@` references resolve to the linked pyramid `index.md` file.
- Parent-child links are always bidirectional — a child references its parent and the parent references the child.
- A pyramid with no parent link (other than root) is an orphan and fails audits.
- Pyramid content never prescribes implementation. Code examples are illustrative only.
- A present `diff.md` sidecar always contains one or more unresolved discrepancies for its sibling pyramid; no-drift state is represented by sidecar absence.

## Relationships

- Parent: [Pyrs](../index.md)
- See Also: [Change Flow](../change-flow/index.md) — the change flow depends on the model's structure and rules

## Constraints

- The pyramid model must not expand to include runtime or executable semantics. Pyramids are inert markdown.
- In identifier form, the segment after `@` must remain simple dot-delimited slugs — no versioning, tagging, or metadata encoding.
- The five required sections must not be extended with additional mandatory sections without updating all commands that read and write pyramids.
- `diff.md` sidecars must not be treated as authoritative pyramid content or persisted as success markers.
