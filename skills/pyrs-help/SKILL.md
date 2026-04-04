---
name: pyrs-help
description: "Invoke pyrs-foundation first. Show available pyramid workflow commands. Triggered by ::help."
---

# Pyramid Help

This skill is activated when the user issues `::help`.

Display the following command reference:

```
Pyramid Workflow Commands
=========================

::                                Load the pyramid workflow context (invokes pyrs-foundation)

::spec P? [...description]        Create a new pyramid or revise an existing one
::ingest P? [...description]      Produce pyramids from existing code (pyramid-first exception)
::mend P? [...description]        Debug an "impossible" issue and patch the pyramid gap

::apply P                         Produce or update code from a pyramid using incremental TDD

::sane P                          Check pyramid P for conceptual alignment with parents
::scan P                          Analyze P's full lineage for structural gaps and missing leaves
::diff P                          Compare code against the pyramid(s) it should mirror and manage per-target unresolved-gap diff.md sidecars

::ls                              List the pyramid hierarchy as a tree (calls out nodes with unresolved-gap diff.md)
::ls describe                     List with 1-sentence descriptions (alias: ::ls d)
::help                            Show this command reference

For commands that take P, P is an @-prefixed pyramid reference unless noted otherwise.
For ::diff P, P can be an @-prefixed pyramid reference or a code path (for example, src/queue/).
When ::spec, ::ingest, ::mend, or ::apply mutates target P and a sibling diff.md exists for P, a same-target diff refresh runs before final output.
Identifier form example: @event-bus.actions
Direct-link form example: @./pyramids/event-bus/actions/index.md
P? means the reference is optional.
"@root" refers to ./pyramids/index.md and "root" is optional inside identifier form.
```
