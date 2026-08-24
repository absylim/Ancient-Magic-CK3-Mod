---
status: stub
audience: dev-design
code_roots:
  - common/lifestyle_perks/01_magic_2_life_magic_tree_perks.txt
  - common/scripted_effects/02_ancient_magic_life_perk_effects.txt
  - common/scripted_guis/magic_scripted_gui_life.txt
  - common/decisions/ai_spellbook/ancient_magic_ai_chiromancy_decisions.txt
  - events/ancient_magic_health_events.txt
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
trello:
---

# Chiromancy

> Stub — healing and life-energy school. Perk tree key is `ancient_magic_life` (not `chiromancy`).

## Design intent

TBD — see **Chiromancy** in [CONTEXT.md](../../CONTEXT.md).

## Perk tree

- **Tree key:** `ancient_magic_life`
- **Perk file:** `common/lifestyle_perks/01_magic_2_life_magic_tree_perks.txt`

## Spells

Link [systems/spell-index.md](../systems/spell-index.md) — heal / greater heal / avatar of life concepts.

## AI behavior

- **Decisions:** `common/decisions/ai_spellbook/ancient_magic_ai_chiromancy_decisions.txt`
- **Health events:** `events/ancient_magic_health_events.txt` (`AMHealth`)
- **AI spellbook healing:** `ancient_magic_ai_spellbook_events.301`–`.303` — see [event-chains/health-and-healing.md](../event-chains/health-and-healing.md)

## Related docs

- [event-chains/health-and-healing.md](../event-chains/health-and-healing.md)
- [systems/spell-index.md](../systems/spell-index.md)
