---
name: pyramid-ls
description: "List the pyramid hierarchy as a tree. Triggered by ::ls:: for structure only, or ::ls describe:: / ::ls d:: to include short descriptions."
---

Before proceeding, read [_foundation.md](../_foundation.md) for the pyramid system rules.

# Pyramid List

This skill is activated when the user issues `::ls::`, `::ls describe::`, or `::ls d::`.

## `::ls::`

Display the pyramid hierarchy as a tree using **dot-delimited identifiers**. Do **not** read the file contents — just show the structure.

Output format:

```
root
├── auth
│   ├── auth.session-management
│   └── auth.oauth
└── event-bus
```

## `::ls describe::` / `::ls d::`

Same tree, but **read each pyramid file** and append a 1-sentence description of its purpose. Keep descriptions short — one line max.

Output format:

```
root — Social media platform for sharing short-form video
├── auth — User authentication and identity management
│   ├── auth.session-management — Stateless session tokens with rolling expiry
│   └── auth.oauth — Third-party OAuth2 provider integration
└── event-bus — Centralized pub/sub messaging for decoupled components
```
