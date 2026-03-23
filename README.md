# Pyrs

*Pronounced "pyres," short for pyramids.*

An AI skill plugin for [Claude Code](https://claude.com/claude-code) and [OpenCode](https://opencode.ai) that introduces a structured, hierarchical approach to building software with LLM agents.

## The Problem

LLM coding agents are powerful, but they have a fundamental limitation: they lack durable, structured understanding of the system they're building. Each conversation starts fresh. Context windows are finite. And the bigger a project gets, the harder it is for an agent to hold the full picture in its head.

This leads to predictable failure modes:

- **Scope creep** — an agent implementing one feature quietly introduces behavior that belongs somewhere else, or that was never asked for
- **Conceptual drift** — as features are added over time, the system's architecture silently diverges from what was intended, and nobody notices until it's a mess
- **Fragile alignment** — the only source of truth is the code itself, but code doesn't explain *why* it exists, what contracts it upholds, or how its pieces relate to each other

Documentation helps, but traditional docs tend to either mirror the code (redundant) or describe aspirational architecture (stale). Neither serves as a reliable bridge between human intent and agent execution.

## The Philosophy

The pyramid workflow is built on a few core ideas:

**Concepts, not code.** Pyramids describe *what* a system does and *why*, never *how*. They define contracts, relationships, and boundaries — not implementations. Code is free to change; concepts are stable. When a pyramid says "messages are delivered at-least-once," the implementation can use a queue, a database, a carrier pigeon — the pyramid doesn't care. It only cares that the contract holds.

**Hierarchy mirrors decomposition.** Software is naturally hierarchical — systems decompose into subsystems, which decompose into components. Pyramids mirror this. The root is the broadest overview. Each level down gets more specific. A parent pyramid owns the contract of how its children relate to each other, while each child owns the contract of what it does internally. This means an agent implementing one piece can understand exactly its scope — and nothing more.

**Strict scoping prevents contamination.** When an agent implements pyramid P, it implements *only* P. Not the parent. Not the children. Not "helpful extras." If a child pyramid doesn't exist yet, the agent leaves an explicit placeholder and moves on. This is enforced — audits will catch scope violations. The result is that each piece of the system is built in isolation against a clear specification, and the specification itself is what ensures the pieces fit together.

**Audits close the loop.** Pyramids aren't static documents that get written and forgotten. The workflow includes explicit audit and review commands that check pyramids against each other (conceptual alignment) and against the code (implementation alignment). Drift is caught, surfaced, and reconciled through conversation with the developer. The system stays honest over time.

**The developer stays in control.** Pyramid commands don't silently make sweeping changes. When drift is found, the agent asks probing questions. When a new pyramid doesn't fit cleanly into the hierarchy, the agent surfaces the tension and asks how to resolve it. The workflow is opinionated about structure but collaborative about decisions.

## How It Works

### The Pyramid Directory

A project using the pyramid workflow has a `./pyramids/` directory at its root. This directory contains `.md` files organized in a hierarchy that mirrors the conceptual structure of the product:

```
pyramids/
├── index.md                    # Broadest product overview
├── auth/
│   ├── index.md                # Auth concept overview
│   ├── session-management.md   # Small, self-contained concept
│   └── oauth/
│       └── index.md            # Deeper nested concept
└── event-bus.md                # Small top-level concept
```

- `./pyramids/index.md` is the root — the broadest overview of what the product is and what its major pieces are
- Concepts with children get a directory with an `index.md` (e.g., `./pyramids/auth/index.md`)
- Small, self-contained concepts get a single file (e.g., `./pyramids/event-bus.md`)
- Nesting is recursive — deeper levels are more granular, but every level remains high-level and conceptual
- Parent pyramids reference their children, and children reference their parent — orphans (unreferenced pyramids) are treated as errors

### Pyramid File Structure

Every pyramid file contains these sections:

| Section | What it captures |
|---------|-----------------|
| **Purpose** | What the concept is and why it exists |
| **Concepts** | Key ideas and behaviors, described in plain language |
| **Contracts** | Behavioral guarantees and invariants — what must be true |
| **Relationships** | Explicit links to parent and child pyramids |
| **Constraints** | Boundaries and prohibitions — what this concept must NOT do |

Pyramids may include abstract code snippets to illustrate a concept, but these are never prescriptive. The real implementation does not need to resemble example code — it only needs to uphold the contracts and respect the constraints.

### Commands

Commands are issued in conversation using `::command context::` syntax (two colons on each side):

| Command | What it does |
|---------|-------------|
| `::new [description]::` | Create a new pyramid, fitting it into the hierarchy with proper parent links |
| `::update P [description]::` | Revise an existing pyramid, surfacing downstream impacts |
| `::audit P::` | Traverse upward from P, checking conceptual alignment across the hierarchy |
| `::review P::` | Compare actual code against the pyramid(s) it should mirror |
| `::implement P::` | Build P's concept in code using incremental test-driven development |
| `::tighten P::` | Update existing code to conform to a revised pyramid, also using TDD |

### The Audit Loop

The real power of the workflow is the feedback loop between pyramids and code:

1. `::new::` or `::update::` defines or revises a concept
2. `::audit::` ensures the concept fits coherently within the hierarchy
3. `::implement::` or `::tighten::` builds or updates the code to match
4. `::review::` catches drift between code and pyramids over time

This loop keeps the system aligned at every level — from high-level product vision down to individual component behavior — across conversations, across agents, and across time.

## Installation

### Claude Code

```sh
claude plugin add zachbutton/pyrs
```

### OpenCode

Clone the repo and symlink the skills into your OpenCode skills path:

```sh
git clone https://github.com/zachbutton/pyrs.git ~/.local/share/pyrs

# Symlink each skill
for skill in ~/.local/share/pyrs/skills/pyramid-*/; do
  ln -s "$skill" ~/.config/opencode/skills/$(basename "$skill")
done
```

Or use the Claude-compatible skills path if you prefer:

```sh
for skill in ~/.local/share/pyrs/skills/pyramid-*/; do
  ln -s "$skill" ~/.claude/skills/$(basename "$skill")
done
```
