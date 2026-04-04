# Sane

## Purpose

Check conceptual alignment by traversing upward from a pyramid to root. Sane is the strict audit — it surfaces every issue at every level without softening findings.

## Concepts

- **Syntax:** `::sane P` — P is an `@`-prefixed pyramid reference, either identifier form (for example, `@auth.oauth` or `@root.auth.oauth`) or a direct pyramid `index.md` link (for example, `@./pyramids/auth/oauth/index.md`). The agent traverses from the referenced pyramid upward to the root pyramid.
- **Upward traversal.** Checks conceptual alignment at each level: orphans, missing sections, scope violations, conceptual misalignment between parent and child, and missing provenance comments or placeholders.
- **Strict reporting.** Audits surface every issue found. They do not stop at the first problem and do not soften or prioritize findings.
- **Read-only.** Sane never modifies pyramids or code. It reports findings and asks the user for direction.

## Contracts

- Sane checks the full upward path from the referenced pyramid to root and reports every issue found.
- Sane does not stop at the first problem.
- Sane does not modify pyramids or code.
- When the user decides how to reconcile a finding, sane hands off to the appropriate command (::spec for pyramid changes, ::apply for code changes).
- Findings are presented with evidence — specific pyramids and sections — not vague summaries.

## Relationships

- Parent: [Verification](../index.md)
- See Also: [Scan](../scan/index.md) — sane checks upward alignment; scan checks structural completeness across the full lineage
- See Also: [Spec](../../pyramid-output/spec/index.md) — sane routes pyramid fixes to spec

## Constraints

- Sane must not silently pass when issues exist.
- Sane must not use git history, blame, or diffs. Current state only.
- Sane must not suggest fixes unprompted. It surfaces problems; the user decides.
