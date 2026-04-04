---
name: pyrs-ls
description: "Invoke pyrs-foundation first. List the pyramid hierarchy as a tree and call out pyramids with unresolved-gap diff sidecars. Triggered by ::ls for structure only, or ::ls describe / ::ls d to include short descriptions."
---

Before proceeding, invoke the pyrs-foundation skill to load the pyramid system rules.

# Pyramid List

This skill is activated when the user issues `::ls`, `::ls describe`, or `::ls d`.

## `::ls`

Display the pyramid hierarchy as a tree using **identifier-form `@` references**. Do **not** read markdown contents — show structure and whether each pyramid has a sibling `diff.md` sidecar indicating unresolved gaps.

Output format:

```
@root
├── @auth [diff]
│   ├── @auth.session-management
│   └── @auth.oauth
└── @event-bus
```

Callout rule:

- If `./pyramids/path/to/pyramid/diff.md` exists next to that pyramid's `index.md`, append ` [diff]` to that node.
- Presence means unresolved reconciliation work still exists for that pyramid.
- Callouts are file-presence based only. Do not parse `diff.md` content.

## `::ls describe` / `::ls d`

Same tree, but **read each pyramid `index.md` file** and append a 1-sentence description of its purpose. Keep descriptions short — one line max.

Output format:

```
@root — Social media platform for sharing short-form video
├── @auth [diff] — User authentication and identity management
│   ├── @auth.session-management — Stateless session tokens with rolling expiry
│   └── @auth.oauth — Third-party OAuth2 provider integration
└── @event-bus — Centralized pub/sub messaging for decoupled components
```
