# Distribution

## Purpose

Packages and distributes pyrs as a plugin for multiple AI coding agent platforms. The system needs to reach users wherever they work — currently Claude Code and OpenCode — without platform-specific logic leaking into the core skill definitions.

## Concepts

- **Platform-agnostic skills.** The skill files (markdown) are identical across platforms. Platform differences are handled entirely at the packaging and installation layer.
- **Claude Code plugin.** Distributed via the Claude Code plugin marketplace using `plugin.json` and `marketplace.json` manifests. Users install through the `/plugin` command.
- **OpenCode integration.** Installed by cloning the repo and running an install script that registers the skills path in OpenCode's config. Skills are discovered via `SKILL.md` files and loaded on demand.
- **Versioning.** The plugin version is tracked in `plugin.json` and corresponds to marketplace releases.

## Contracts

- Skill files are platform-agnostic — no platform-specific conditionals or branching within skill content.
- The OpenCode install script merges with existing configuration rather than overwriting it.
- Plugin metadata (`plugin.json`, `marketplace.json`) accurately reflects the current version and authorship.
- The README documents `::` standalone as a way to make the agent aware of PYRS rules without performing a pyramid operation.

## Relationships

- Parent: [Pyrs](../index.md)

## Constraints

- Platform-specific packaging must not influence skill content. Skills must read identically regardless of which platform loads them.
- Distribution must not introduce runtime dependencies. Pyrs is pure markdown consumed by the host agent.
