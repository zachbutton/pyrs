# PYRS

*Pronounced "pyres," short for pyramids.*

**P**yramidal **Y**ield-**R**eady **S**pecifications

```
               ▲
              ▲ ▲
             ▲   ▲
            ▲ ▲ ▲ ▲
           ▲       ▲
          ▲ ▲     ▲ ▲
         ▲   ▲   ▲   ▲
        ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲
       ▲               ▲
      ▲ ▲   P.Y.R.S   ▲ ▲
     ▲   ▲           ▲   ▲
    ▲ ▲ ▲ ▲         ▲ ▲ ▲ ▲
   ▲       ▲       ▲       ▲
  ▲ ▲     ▲ ▲     ▲ ▲     ▲ ▲
 ▲   ▲   ▲   ▲   ▲   ▲   ▲   ▲
▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲
```

An AI skill plugin for [Claude Code](https://claude.com/claude-code) and [OpenCode](https://opencode.ai) that introduces a structured, hierarchical approach to building software with LLM agents.

## Table of Contents

- [What is PYRS?](#what-is-pyrs)
- [The Problem](#the-problem)
- [The Philosophy](#the-philosophy)
- [How It Works](#how-it-works)
  - [The Pyramid Directory](#the-pyramid-directory)
  - [Pyramid File Structure](#pyramid-file-structure)
  - [Pyramid Identifiers](#pyramid-identifiers)
  - [Commands](#commands)
  - [Placeholders](#placeholders)
  - [The Audit Loop](#the-audit-loop)
- [Example Walkthrough](#example-walkthrough)
- [Bootstrapping an Existing Project](#bootstrapping-an-existing-project)
- [Adoption Strategies](#adoption-strategies)
  - [Full Coverage (new projects)](#full-coverage-new-projects)
  - [Sparse Coverage (existing projects)](#sparse-coverage-existing-projects)
  - [Meta Pyramid Repos (codebases you don't control)](#meta-pyramid-repos-codebases-you-dont-control)
- [Source Control](#source-control)
- [Installation](#installation)
  - [Claude Code](#claude-code)
  - [OpenCode](#opencode)

## What is PYRS?

Persistent context for AI coding agents. As you work with an agent, PYRS produces simple markdown files ("pyramids") that capture what your system does and why. These pyramids accumulate as a natural byproduct of the conversations you're already having. They're not documentation you maintain; they're artifacts that keep every future agent session aligned with your architecture.

**Without PYRS** — no matter how thorough your planning phase is, context doesn't survive the session:

```mermaid
graph TD
    A1["Agent · Session 1"] --> C["Code"]
    A2["Agent · Session 2"] --> C
    A3["Agent · Session N"] --> C
    C --> X["scope creep · drift · decay"]

    style X fill:#e74c3c,stroke:#c0392b,color:#fff
```

**With PYRS** — pyramids persist across sessions, drive tests, tests drive code:

```mermaid
graph TD
    B1["Agent · Session 1"] --> P["Pyramids<br/><i>source of truth · persistent contracts</i>"]
    B2["Agent · Session 2"] --> P
    B3["Agent · Session N"] --> P
    P --> T["Tests · derived from contracts"]
    T --> C["Code · passes tests"]
    C -.-> P

    style P fill:#4a90d9,stroke:#2c5f8a,color:#fff
    style T fill:#f5a623,stroke:#c4841c,color:#fff
    style C fill:#7ed321,stroke:#5a9e18,color:#fff
```

**P**yramidal **Y**ield-**R**eady **S**pecifications

Created by [Zach Button](https://linkedin.com/in/zachbutton/)

## The Problem

LLM coding agents are powerful, but they have a fundamental limitation: they lack durable, structured understanding of the system they're building. Each conversation starts fresh. Context windows are finite. And the bigger a project gets, the harder it is for an agent to hold the full picture in its head.

This leads to predictable failure modes:

- **Scope creep** — an agent implementing one feature quietly introduces behavior that belongs somewhere else, or that was never asked for
- **Conceptual drift** — as features are added over time, the system's architecture silently diverges from what was intended, and nobody notices until it's a mess
- **Fragile alignment** — the only source of truth is the code itself, but code doesn't explain *why* it exists, what contracts it upholds, or how its pieces relate to each other

Documentation helps, but traditional docs tend to either mirror the code (redundant) or describe aspirational architecture (stale). Neither serves as a reliable bridge between human intent and agent execution.

## The Philosophy

The pyramid workflow is built on a few core ideas:

**Concepts, not code.** Pyramids describe *what* a system does and *why*, never *how*. They define contracts, relationships, and boundaries — not implementations. Code is free to change; concepts are stable. When a pyramid says "messages are delivered at-least-once," the implementation can use a queue, a database, a carrier pigeon. The pyramid doesn't care. It only cares that the contract holds.

**Hierarchy mirrors decomposition.** Software is naturally hierarchical: systems decompose into subsystems, which decompose into components. Pyramids mirror this. The root is the broadest overview. Each level down gets more specific. A parent pyramid owns the contract of how its children relate to each other, while each child owns the contract of what it does internally. This means an agent implementing one piece can understand exactly its scope — and nothing more.

**The agent stays focused so you don't have to.** When an agent implements a pyramid, it builds *only* what the pyramid describes. Not the parent. Not the children. Not "helpful extras." If a dependency doesn't exist yet, the agent leaves a placeholder and moves on. You don't have to police scope; the pyramid does that for the agent.

**You can check alignment anytime.** Pyramids aren't static documents that get written and forgotten. Quick audit and review commands let you check how pyramids relate to each other and whether the code still matches. When drift is found, it's surfaced for you to decide what to do. The agent asks, it doesn't assume.

**Commands are just natural language.** The `::command::` syntax tells the plugin which strategy to load, but everything inside is conversational. Describe what you want in plain English. There's nothing to memorize; `::help::` shows the full list.

**The developer stays in control.** Pyramid commands don't silently make sweeping changes. When drift is found, the agent asks probing questions. When a new pyramid doesn't fit cleanly into the hierarchy, the agent surfaces the tension and asks how to resolve it. The workflow is opinionated about structure but collaborative about decisions.

## How It Works

### The Pyramid Directory

PYRS stores pyramids in a `./pyramids/` directory at the project root. These are plain markdown files organized in a hierarchy that mirrors the conceptual structure of the product:

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
- Nesting is recursive; deeper levels are more granular, but every level remains high-level and conceptual
- Parent pyramids reference their children, and children reference their parent. The plugin keeps these links consistent

### Pyramid File Structure

Every pyramid file contains these sections:

| Section | What it captures |
|---------|-----------------|
| **Purpose** | What the concept is and why it exists |
| **Concepts** | Key ideas and behaviors, described in plain language |
| **Contracts** | Behavioral guarantees and invariants — what must be true |
| **Relationships** | Explicit links to parent and child pyramids, plus See Also cross-references to related concepts |
| **Constraints** | Boundaries and prohibitions — what this concept must NOT do |

Pyramids may include abstract code snippets to illustrate a concept, but these are never prescriptive. The real implementation does not need to resemble example code — it only needs to uphold the contracts and respect the constraints.

**When does a concept need its own pyramid?** When it has contracts that can be violated independently. If a behavior is just a detail of a parent's contract, it belongs in the parent. If it has its own guarantees, boundaries, or relationships — things an agent could get wrong in isolation — it's a pyramid.

### Pyramid Identifiers

Commands reference pyramids using dot-delimited identifiers that resolve to file paths:

- `event-bus` → `./pyramids/event-bus.md` or `./pyramids/event-bus/index.md`
- `event-bus.actions` → `./pyramids/event-bus/actions.md` or `./pyramids/event-bus/actions/index.md`
- `root` → `./pyramids/index.md` — use this when you want to target the top-level pyramid itself (e.g., `::audit root::`)
- `root` is optional as a prefix — `root.event-bus.actions` and `event-bus.actions` resolve to the same pyramid, so you can omit it

### Commands

Commands are issued in conversation using `::command context::` syntax (two colons on each side). Each command loads a specific strategy; the agent handles the rest. `P` is a pyramid identifier in dot notation (e.g., `task-queue.retry`). `P?` means the identifier is optional.

| Command | What it does |
|---------|-------------|
| `::new P? [...description]::` | Create a new pyramid, fitting it into the hierarchy with proper parent links |
| `::update P? [...description]::` | Revise an existing pyramid, surfacing downstream impacts |
| `::implement P::` | Build P's concept in code using incremental test-driven development |
| `::tighten P::` | Update existing code to conform to a revised pyramid, also using TDD |
| `::audit P::` | Traverse upward from P, checking conceptual alignment across the hierarchy |
| `::review P::` | Compare actual code against the pyramid(s) it should mirror — P can also be a code path (e.g., `src/queue/`) |
| `::survey P::` | Analyze P and its full lineage (ancestors + descendants) for structural gaps and missing leaves |
| `::post-mortem P? [...description]::` | Debug an issue that should have been impossible given the pyramids, then patch the gap |
| `::bootstrap P? [...description]::` | Produce pyramids from existing code — the one exception to pyramid-first |
| `::ls::` | List the pyramid hierarchy as a tree; `::ls describe::` to include descriptions |
| `::help::` | Show the command reference |

If you describe a code change without using a `::` command, the plugin will gently redirect you into the pyramid workflow. It'll help you figure out which pyramid to create or update first.

### Placeholders

When `::implement::` or `::tighten::` encounters an unbuilt dependency — whether a child pyramid or a sibling referenced via See Also — it leaves an explicit placeholder in the code: `// PYRS_TODO: ./pyramids/[path]`. These placeholders include meaningful runtime logging so missing pieces are visible during execution, not silently absent. Once the dependency is created and implemented, the placeholder gets replaced.

### The Audit Loop

The real power of the workflow is the feedback loop between pyramids and code:

1. `::new::` or `::update::` defines or revises a concept
2. `::audit::` ensures the concept fits coherently within the hierarchy
3. `::implement::` or `::tighten::` builds or updates the code to match
4. `::review::` catches drift between code and pyramids over time

This loop keeps the system aligned at every level — from high-level product vision down to individual component behavior — across conversations, across agents, and across time.

## Example Walkthrough

Here's how the pyramid workflow plays out across a real project. Each step below happens in a **fresh session** — separate conversation, different day, even a different agent. Notice that every interaction is a one-liner; the agent does the heavy lifting.

---

**Session 1** — Define the concept

```
::new task-queue a task queue that processes background jobs::
```

The agent asks where this fits in the hierarchy, you discuss scope and contracts, and it writes the pyramid.

*Result: `./pyramids/task-queue.md` exists with Purpose, Concepts, Contracts, Relationships, and Constraints.*

---

**Session 2** — Build it *(fresh context)*

```
::implement task-queue::
```

The agent reads `task-queue.md`. That's all it needs. It builds the implementation via incremental TDD. Child concepts referenced but not yet defined get `// PYRS_TODO` placeholders with runtime logging.

---

**Session 3** — Decompose further *(fresh context)*

```
::new task-queue.retry retry logic with exponential backoff::
```

The agent creates `./pyramids/task-queue/retry.md` as a child pyramid and updates the parent to reference it.

---

**Session 4** — Build the child *(fresh context)*

```
::implement task-queue.retry::
```

The agent reads `retry.md`, implements via TDD, and replaces the `PYRS_TODO` placeholder left in Session 2.

---

**Session 5** — Revise *(fresh context)*

```
::update task-queue dead-letter support after max retries::
```

The agent revises `task-queue.md`. It surfaces that this impacts `retry.md` and asks how to reconcile before making changes.

---

**Session 6** — Tighten to match *(fresh context)*

```
::tighten task-queue::
```

Updates existing task-queue code to conform to the revised pyramid via TDD.

---

**Later** — Maintenance *(fresh context)*

```
::review task-queue::
```

Catches drift between code and pyramids.

```
::audit task-queue::
```

Verifies conceptual alignment up the hierarchy.

---

Every session starts cold. The pyramids are the memory.

**In practice:** When something breaks that shouldn't have been possible, `::post-mortem::` traces the failure and patches the pyramid gap: a missing contract, an under-specified constraint, a cross-pyramid interaction nobody accounted for. Then a fresh agent runs `::tighten::` and independently fixes the bug it never saw, because the pyramid now describes the world correctly.

## Bootstrapping an Existing Project

If you have an existing codebase and want to adopt PYRS, `::bootstrap::` reverses the normal flow — it reads code and produces pyramids. This is the **one exception** to the pyramid-first rule.

```
::bootstrap::
```

The agent surveys your codebase, proposes a pyramid hierarchy, and, after your confirmation, writes the pyramids top-down. Each pyramid is conceptual, not a code description. Once bootstrap is complete, the normal workflow applies: all future changes flow through pyramids.

You can also bootstrap a specific area:

```
::bootstrap auth — focus on session handling and OAuth::
```

Or use it after writing code manually. Bootstrap captures the concept so the pyramid artifact exists for future agents:

```
::bootstrap task-queue.retry::
```

## Adoption Strategies

### Full Coverage (new projects)

When starting a project from scratch, pyramids grow naturally alongside the code. `::new::` before `::implement::` becomes the natural rhythm: you describe what you want, the agent writes the pyramid, then builds the code. The pyramids are artifacts of conversations you're already having, not extra work.

### Sparse Coverage (existing projects)

You don't need to bootstrap your entire codebase. Even a handful of pyramids covering your most drift-prone areas prevents the worst architectural decay. Add them incrementally as you touch areas. `::bootstrap::` what matters, leave the rest. A sparse set of pyramids still gives agents meaningful guardrails in the areas that count.

### Meta Pyramid Repos (codebases you don't control)

If your projects live in `~/code/*`, you can make `~/code/` itself a private repo with its own `./pyramids/` directory. The pyramids describe concepts across multiple codebases — sparse by nature, growing with your understanding of the systems.

This is especially useful at work when your company doesn't use PYRS. Your agents still get durable context across sessions, and as a side effect, `./pyramids/` accumulates tribal knowledge: how systems connect, why things are the way they are, what the gotchas are. That knowledge lives in portable markdown files you can share with your team anytime.

## Source Control

The `./pyramids/` directory should be committed to version control alongside your code. Pyramid changes are changes — treat them like any other commit. The recommended convention is to use a `docs:` prefix for pyramid-only changes (e.g., `docs: add task-queue.retry pyramid`). The same applies to meta pyramid repos: commit pyramids alongside whatever they describe.

## Installation

### Claude Code

From within Claude Code:

```
/plugin marketplace add zachbutton/pyrs
/plugin install pyrs@zachbutton-pyrs
```

### OpenCode

Clone the repo and add the skills path to your OpenCode config (`.opencode.json` in your project or `~/.config/opencode/config.json`):

```sh
git clone https://github.com/zachbutton/pyrs.git ~/.local/share/pyrs
```

```json
{
  "options": {
    "skills_paths": ["~/.local/share/pyrs/skills"]
  }
}
```

Skills are loaded on demand, not all at once. OpenCode scans the path for `SKILL.md` files and only reads a skill's full instructions when a task matches its description.
