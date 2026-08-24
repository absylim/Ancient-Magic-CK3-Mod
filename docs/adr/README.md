# Architecture Decision Records (ADR)

ADRs document **irreversible, cross-cutting** choices that affect multiple systems. They are rare — most design lives in `mechanics/`, `features/`, or `event-chains/`.

## When to write an ADR

Write an ADR when a decision:

- Is hard to reverse (e.g. bitmask progression vs trait-flag unlocks)
- Spans multiple folders (`common/scripted_effects/`, `events/`, GUI)
- Future contributors will ask "why did we do it this way?"

Do **not** write an ADR for routine perk balance, single event chains, or school-specific spells.

## Format

Create `docs/adr/NNNN-short-title.md` with:

- **Context** — what problem we faced
- **Decision** — what we chose
- **Consequences** — tradeoffs and follow-up work

## Index

_No ADRs yet._
