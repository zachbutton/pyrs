---
name: pyrs-help
description: "Show available pyramid workflow commands. Triggered by ::help::."
---

# Pyramid Help

This skill is activated when the user issues `::help::`.

Display the following command reference:

```
Pyramid Workflow Commands
=========================

::new P? [...description]::     Create a new pyramid, placing it in the hierarchy
::update P? [...description]::  Revise an existing pyramid, surfacing downstream impacts
::implement P::                 Implement pyramid P using incremental TDD
::tighten P::                   Update existing code to conform to a revised pyramid
::audit P::                     Check pyramid P for conceptual alignment with parents
::review P::                    Compare code against the pyramid(s) it should mirror
::survey P::                    Analyze P's full lineage for structural gaps and missing leaves
::post-mortem P? [...description]::  Debug an "impossible" issue and patch the pyramid gap
::bootstrap P? [...description]::    Produce pyramids from existing code (pyramid-first exception)

::ls::                          List the pyramid hierarchy as a tree
::ls describe::                 List with 1-sentence descriptions (alias: ::ls d::)
::help::                        Show this command reference

P is a dot-delimited pyramid identifier (e.g., event-bus.actions).
P? means the identifier is optional.
"root" refers to ./pyramids/index.md and is optional as a prefix.
```
