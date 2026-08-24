---
status: stub
audience: dev-design
code_roots:
  - common/scripted_effects/01_ancient_magic_spell_effects.txt
  - common/lifestyle_perks/
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
trello:
---

# Spell Index

> Stub — **future** master cross-school spell table. School docs link here; do not duplicate full spell lists locally.

## Overview

Single table mapping spell loc keys → school → perk → effect script → mana cost. Populate incrementally (chiromancy pilot per [docs/README.md](../README.md)).

## File index

| Column (planned) | Source |
|------------------|--------|
| Spell name / loc key | `ancient_magic_game_concepts_l_english.yml`, spell loc |
| School | Perk tree / GUI file |
| Effect | `common/scripted_effects/01_ancient_magic_spell_effects.txt` |
| Cost | `common/script_values/01_ancient_magic_spell_cost_values.txt` |

## Spell table

| Spell | School | Type | Loc key | Notes |
|-------|--------|------|---------|-------|
| TBD | TBD | TBD | TBD | Seed row in chiromancy pilot |

## Related docs

- [schools/](../schools/)
- [mechanics/spellbook-and-auras.md](../mechanics/spellbook-and-auras.md)
