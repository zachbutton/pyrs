# Scan

## Purpose

Analyze a pyramid and its full lineage (ancestors and descendants) for structural gaps and completeness issues. Scan focuses on the shape and coverage of the hierarchy, not on code alignment.

## Concepts

- **Syntax:** `::scan P` — P is an `@`-prefixed pyramid reference, either identifier form (for example, `@auth.oauth` or `@root.auth.oauth`) or a direct pyramid `index.md` link (for example, `@./pyramids/auth/oauth/index.md`). The agent analyzes the referenced pyramid and its full lineage in both directions.
- **Structural analysis.** Detects missing leaf pyramids, under-specified areas, cross-cutting concerns without See Also links, and stale TODOs.
- **Lineage scope.** Examines both upward (ancestors) and downward (descendants) from the referenced pyramid, prioritizing gaps by impact.
- **Read-only.** Scan never modifies pyramids or code. It reports structural findings and asks the user for direction.

## Contracts

- Scan examines the full lineage (up and down) from the referenced pyramid.
- Gaps are prioritized by impact.
- Scan does not modify pyramids or code.
- When the user decides how to address a gap, scan hands off to the appropriate command (::spec for new or revised pyramids).
- Findings are presented with evidence — specific pyramids and missing areas — not vague summaries.

## Relationships

- Parent: [Verification](../index.md)
- See Also: [Sane](../sane/index.md) — scan checks structural completeness; sane checks conceptual alignment upward
- See Also: [Ingest](../../pyramid-output/ingest/index.md) — after ingest, scan can verify the new hierarchy's completeness

## Constraints

- Scan must not silently pass when gaps exist.
- Scan focuses on structural completeness, not code alignment. Use ::diff for code comparison.
- Scan must not suggest fixes unprompted. It surfaces gaps; the user decides.
