# Change Flow

## Purpose

Defines the pyramid-first discipline: all changes to code must originate in pyramids. This flow is what keeps agent behavior aligned with human intent across sessions. Without it, agents drift and pyramids become stale documentation.

## Concepts

- **Pyramid-first.** The path is always: describe the change conceptually (spec), verify alignment (sane), then apply the code (apply). Code is never modified outside this flow.
- **Ingest exception.** When capturing pyramids from existing code, ingest reverses the flow — it reads code and produces pyramids. Use it when adopting pyrs, when capturing manually-written code, or when re-ingesting code that has evolved.
- **Strict scoping.** Implementation is bounded by the target pyramid. An agent implementing a pyramid builds only what that pyramid describes. Dependencies that don't exist yet get placeholders, not implementations.
- **Provenance tracking.** Code and tests generated from pyramids carry provenance comments (`// PYRS: identifier`) so the connection between concept and code is traceable. Unbuilt dependencies get `// PYRS_TODO` placeholders with runtime logging.
- **Opt-out for code markers.** A pyramid (or any ancestor) can include a constraint prohibiting code markers. This opt-out is inherited by all descendants and causes provenance comments and placeholders to be omitted entirely.
- **The audit loop.** The feedback loop between pyramids and code: define or revise a concept (spec), check for hierarchy fit (sane), apply the code (apply), check for drift (diff). This loop runs continuously across sessions.

## Contracts

- All code changes flow through `apply` — this is the only route from pyramid to code.
- Implementation scope is strictly bounded by the target pyramid. No parent scope, no child scope, no undocumented extras.
- Unbuilt dependencies produce `PYRS_TODO` placeholders with meaningful runtime logging, unless the pyramid opts out of code markers.
- Code generated from a pyramid carries `PYRS: identifier` provenance comments, unless the pyramid opts out of code markers.
- The code marker opt-out is inherited: a parent's constraint applies to all descendants but not to other branches.
- When drift or ambiguity is found during any phase, the agent asks the user rather than assuming intent.

## Relationships

- Parent: [Pyrs](../index.md)
- See Also: [Pyramid Model](../pyramid-model/index.md) — the change flow operates on the structures defined by the model
- See Also: [Commands](../commands/index.md) — commands are the mechanism through which the change flow is executed

## Constraints

- The change flow must not allow direct code modification outside of `apply`. Ingest does not modify code — it only reads code and produces pyramids.
- Placeholders must not silently degrade — they must produce visible runtime output so missing pieces are noticed, not ignored.
- The agent must never silently resolve ambiguity during any phase of the flow. Probing over assuming is non-negotiable.
