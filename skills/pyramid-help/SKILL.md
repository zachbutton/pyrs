---
name: pyramid-help
description: "Show available pyramid workflow commands. Triggered by ::help::."
---

# Pyramid Help

This skill is activated when the user issues `::help::`.

Display the following command reference:

```
Pyramid Workflow Commands
=========================

::ls::                    List the pyramid hierarchy as a tree
::ls describe::           List with 1-sentence descriptions (alias: ::ls d::)
::help::                  Show this command reference

::new [description]::     Create a new pyramid, placing it in the hierarchy
::update P [description]::  Revise an existing pyramid
::audit P::               Check pyramid P for conceptual alignment with parents
::review P::              Compare code against the pyramid(s) it should mirror
::implement P::           Implement pyramid P using incremental TDD
::tighten P::             Update existing code to conform to a revised pyramid
::post-mortem [issue]::   Debug an "impossible" issue and patch the pyramid gap

P can be a dot-delimited identifier (e.g., event-bus.actions).
"root" refers to ./pyramids/index.md and is optional as a prefix.
```
