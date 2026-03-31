# Creation

## Purpose

Commands that produce new pyramids. These are the entry points for introducing new concepts into the hierarchy, whether from a user's description of something that doesn't exist yet or from existing code that needs to be captured.

## Concepts

- **New (`::new P? [...description]::`)** — creates a pyramid for a concept that doesn't exist yet. The agent asks probing questions about where it fits in the hierarchy, writes the pyramid with all required sections, and immediately updates the parent to reference it. If the identifier is omitted, the agent works with the user to determine placement.
- **Bootstrap (`::bootstrap P? [...description]::`)** — produces pyramids from existing code. The agent surveys the codebase (or a targeted area), proposes a hierarchy, and writes pyramids top-down after user confirmation at each level. Bootstrap abstracts upward from implementation to concepts — a bootstrap pyramid should be indistinguishable from one created via new. Bootstrap is read-only with respect to code.
- **Hierarchy placement is deliberate.** Both commands require the new pyramid to fit coherently within the existing hierarchy. The agent surfaces tensions rather than silently placing a pyramid where it doesn't belong.

## Contracts

- New pyramids are always written with all five required sections (Purpose, Concepts, Contracts, Relationships, Constraints).
- The parent pyramid is updated to reference the new child immediately after creation.
- Bootstrap never modifies code — it only reads code and produces pyramids.
- Bootstrap pyramids are conceptual, not code descriptions. They capture what and why, never how.
- Bootstrap proposes the hierarchy and requires explicit user confirmation before writing any pyramids.
- When placement is ambiguous, the agent asks the user rather than guessing.

## Relationships

- Parent: [Commands](./index.md)
- See Also: [Maintenance](./maintenance.md) — update evolves existing pyramids; creation produces new ones

## Constraints

- Creation commands must not modify existing pyramids beyond adding child references to parents.
- Bootstrap must not invent contracts the code does not currently uphold. It captures what is, not what should be.
- Neither command may produce code or tests. Their output is exclusively pyramids.
