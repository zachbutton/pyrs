# Verification

## Purpose

Commands that check alignment — between pyramids and their hierarchy, between pyramids and code, and across the structural completeness of the tree. These are the system's quality gates, usable at any time to detect drift before it compounds.

## Concepts

- **Audit (`::audit P::`)** — traverses upward from P to root, checking conceptual alignment at each level. Looks for orphans, missing sections, scope violations, conceptual misalignment between parent and child, and missing provenance comments or placeholders. Audits are strict — they surface every issue without softening findings.
- **Review (`::review P::`)** — compares actual code against the pyramid(s) it should mirror. Detects code doing more than the pyramid describes, code doing less than it requires, stale PYRS_TODO placeholders, and missing or inaccurate provenance comments. P can also be a code path. Review surfaces discrepancies and asks the user how to reconcile.
- **Survey (`::survey P::`)** — analyzes P and its full lineage (ancestors and descendants) for structural gaps: missing leaf pyramids, under-specified areas, cross-cutting concerns without See Also links, and stale TODOs. Survey focuses on structural completeness, not code.
- **Read-only.** All verification commands are non-destructive. They report findings but never modify pyramids or code.
- **Handoff to the change flow.** When a verification command surfaces findings and the user decides how to reconcile, the command must route the user to the appropriate pyramid workflow command — `::update::` for pyramid changes, `::tighten::` for code changes, `::new::` for missing pyramids. The verification command never performs the resolution itself. This closes the loop between verification and the change flow.

## Contracts

- Audit checks the full upward path from P to root and reports every issue found. It does not stop at the first problem.
- Review compares current code against current pyramids. It does not use git history as a source of truth.
- Survey examines the full lineage (up and down) from P and prioritizes gaps by impact.
- No verification command modifies pyramids or code. They report findings and ask the user for direction.
- When the user decides how to reconcile a finding, the verification command hands off to the appropriate change flow command (`::update::`, `::tighten::`, `::new::`). It does not perform the resolution directly.
- Findings are presented with evidence — specific pyramids, sections, or code locations — not vague summaries.

## Relationships

- Parent: [Commands](./index.md)
- See Also: [Change Flow](../change-flow.md) — verification commands are part of the audit loop

## Constraints

- Verification commands must not silently pass when issues exist. Leniency in audits defeats the purpose of the system.
- Verification must not use git history, blame, or diffs. The current state of pyramids and code is the only input.
- Verification must not suggest fixes unprompted. It surfaces problems; the user decides what to do.
