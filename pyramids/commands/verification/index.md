# Verification

## Purpose

Commands that check alignment — between pyramids and their hierarchy, between pyramids and code, and across the structural completeness of the tree. These are the system's quality gates, usable at any time to detect drift before it compounds.

## Concepts

- **Sane** traverses upward from a pyramid to root, checking conceptual alignment at each level.
- **Scan** analyzes a pyramid and its full lineage for structural gaps and completeness issues.
- **Diff** compares actual code against the pyramids it should mirror.
- **Output profile.** Sane and scan are read-only. Diff manages targeted `diff.md` sidecars as actionable artifacts: create or update when discrepancies exist, delete when clean.
- **Refresh integration.** Mutation commands may run same-target diff refresh passes when a sibling `diff.md` exists, keeping verification artifacts current after reconciliation work.
- **Handoff to the change flow.** When a verification command surfaces findings and the user decides how to reconcile, the command routes the user to the appropriate command — ::spec for pyramid changes, ::apply for code changes. The verification command never performs the resolution itself.

## Contracts

- Sane checks the full upward path from the referenced pyramid to root and reports every issue found. It does not stop at the first problem.
- Diff compares current code against current pyramids. It does not use git history as a source of truth.
- Scan examines the full lineage (up and down) from the referenced pyramid and prioritizes gaps by impact.
- Verification commands never modify code. Sane and scan do not modify pyramid files; diff may create, update, or delete only targeted `diff.md` sidecars.
- Diff refreshes triggered by mutation commands obey the same target scope and sidecar lifecycle as direct diff invocations.
- When the user decides how to reconcile a finding, the verification command hands off to the appropriate command (::spec, ::apply). It does not perform the resolution directly.
- Findings are presented with evidence — specific pyramids, sections, or code locations — not vague summaries.

## Relationships

- Parent: [Commands](../index.md)
- Children:
  - [Sane](./sane/index.md) — check conceptual alignment upward to root
  - [Scan](./scan/index.md) — check structural completeness across lineage
  - [Diff](./diff/index.md) — compare code against pyramids and manage per-target unresolved-drift sidecars
- See Also: [Change Flow](../../change-flow/index.md) — verification commands are part of the audit loop

## Constraints

- Verification commands must not silently pass when issues exist. Leniency in audits defeats the purpose of the system.
- Verification must not use git history, blame, or diffs. The current state of pyramids and code is the only input.
- Verification commands must not modify pyramid `index.md` files.
- Verification output must not persist `diff.md` sidecars for clean targets. No-drift state is represented by sidecar absence.
- Verification must not suggest fixes unprompted. It surfaces problems; the user decides what to do.
