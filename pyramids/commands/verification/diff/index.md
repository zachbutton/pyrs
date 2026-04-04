# Diff

## Purpose

Compare actual code against the pyramids it should mirror. Diff detects drift between what the pyramids describe and what the code actually does, then records unresolved drift in per-target reports that exist only while reconciliation is pending.

## Concepts

- **Syntax:** `::diff P` — P is an `@`-prefixed pyramid reference (identifier form like `@auth.oauth` or direct pyramid `index.md` link like `@./pyramids/auth/oauth/index.md`) or a code path. The agent compares the code against the pyramid(s) it should implement.
- **Discrepancy detection.** Finds code doing more than the pyramid describes, code doing less than it requires, stale PYRS_TODO placeholders, and missing or inaccurate provenance comments.
- **Outstanding-gap sidecars.** For each targeted pyramid with discrepancies, diff writes or updates a sibling `diff.md` file (for example, `./pyramids/auth/oauth/diff.md`) that lists unresolved gaps between implementation and spec. If no discrepancies remain for a target, diff deletes any existing sidecar.
- **Post-mutation refresh entrypoint.** Commands that mutate pyramids or code may invoke a same-target diff pass when a sibling `diff.md` already exists, so reports are immediately refreshed after reconciliation work.
- **Reconciliation handoff.** Surfaces discrepancies and asks the user how to reconcile — route to ::spec if the pyramid needs updating, or ::apply if the code needs updating.
- **Code-safe audit command.** Diff never modifies implementation code or pyramid `index.md` files. Its only filesystem output is managing targeted `diff.md` audit sidecars.

## Contracts

- Diff compares current code against current pyramids. It does not use git history as a source of truth.
- Diff creates or updates `./pyramids/path/to/target/diff.md` only when one or more unresolved discrepancies exist for that target.
- Diff removes an existing target `diff.md` when the latest comparison finds zero discrepancies.
- A `diff.md` report is always actionable: it is never empty and never a no-drift success message.
- When invoked as post-mutation refresh, diff preserves the same target scope and sidecar lifecycle rules as user-invoked diff.
- Diff does not modify code or pyramid `index.md` files.
- When the user decides how to reconcile, diff hands off to the appropriate command (::spec for pyramid changes, ::apply for code changes).
- Findings are presented with evidence — specific code locations and pyramid sections — not vague summaries.

## Relationships

- Parent: [Verification](../index.md)
- See Also: [Apply](../../code-output/apply/index.md) — diff detects code drift; apply fixes it
- See Also: [Spec](../../pyramid-output/spec/index.md) — diff detects pyramid drift; spec fixes it
- See Also: [Ls](../../utility/ls/index.md) — ls surfaces where diff reports exist in the pyramid tree

## Constraints

- Diff must not silently pass when discrepancies exist.
- Diff must not use git history, blame, or diffs as source of truth. Current state only.
- Diff must not write report files outside the targeted pyramid directories.
- Diff must not leave behind empty or no-drift `diff.md` files.
- Diff must not suggest fixes unprompted. It surfaces discrepancies; the user decides.
