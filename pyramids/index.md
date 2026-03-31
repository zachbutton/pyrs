# Pyrs

## Purpose

Pyrs gives AI coding agents persistent architectural memory. Without it, every agent session starts cold — no knowledge of what the system is, why it's shaped the way it is, or what contracts it upholds. Pyrs solves this by producing simple markdown artifacts (pyramids) that capture concepts, contracts, and relationships. These artifacts accumulate as a natural byproduct of working with the agent and serve as the source of truth across sessions, agents, and time.

## Concepts

- **Pyramids are the memory.** They persist between sessions and provide the context an agent needs to work correctly without human re-explanation. They are not documentation — they are operational artifacts that drive implementation and verification.
- **Concepts, not code.** Pyramids describe what a system does and why, never how. Implementations can change freely as long as contracts hold. This separation keeps pyramids stable even as code evolves.
- **Hierarchy mirrors decomposition.** The pyramid hierarchy reflects how a system decomposes conceptually. Parents own the relationships between children; children own their internal behavior. This scoping discipline is what prevents agents from exceeding their mandate.
- **The agent stays focused.** When implementing a pyramid, the agent builds only what that pyramid describes. No parent behavior. No child behavior. No helpful extras. Unbuilt dependencies get placeholders, not guesses.
- **Alignment is verifiable.** Commands let users check pyramid-to-pyramid alignment (audit), pyramid-to-code alignment (review), and structural completeness (survey) at any time. Drift is surfaced, not silently resolved.

## Contracts

- Every pyramid links to its parent and is referenced by its parent. Orphans are invalid.
- All code changes flow through pyramids: describe the change conceptually first, then implement. Bootstrap is the sole exception.
- Pyramids are conceptual — they never prescribe implementation details. Code must match the concept, not any example syntax.
- Implementation scope is strictly bounded by the target pyramid. No parent scope, no child scope, no undocumented behavior.
- When drift or ambiguity is found, the agent asks the user — it does not assume intent.

## Relationships

- Children:
  - [Pyramid Model](./pyramid-model/index.md) — what pyramids are and how they're structured
  - [Change Flow](./change-flow.md) — the pyramid-first discipline governing how changes happen
  - [Commands](./commands/index.md) — the command vocabulary users interact with
  - [Distribution](./distribution.md) — multi-platform plugin packaging

## Constraints

- Pyrs must not become a code generation framework. It is a specification and alignment system.
- Pyramids must never be treated as documentation that supplements code — they are the source of truth that precedes code.
- The system must not silently resolve ambiguity. User confirmation is required for any decision that could cascade.
- This repo should NOT use provenance comments since it's self-referential and is .md files, not code
