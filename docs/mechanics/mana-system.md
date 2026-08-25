---
status: stub
audience: dev-design
code_roots:
  - common/script_values/01_ancient_magic_mana_system_values.txt
  - common/on_action/01_ancient_magic_mana_system_on_actions.txt
  - common/scripted_effects/01_ancient_magic_spell_system_effects.txt
  - common/scripted_effects/01_ancient_magic_effects.txt
  - common/script_values/01_ancient_magic_spell_cost_values.txt
  - common/scripted_guis/01_ancient_magic_hud_sguis.txt
  - common/scripted_triggers/01_ancient_magic_spell_triggers.txt
  - common/game_rules/01_ancient_magic_game_rules.txt
  - gui/custom_gui/mana_system_hud.gui
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
  - localization/english/magic_gui_l_english.yml
  - localization/english/ancient_magic_l_english.yml
trello:
---

# Mana System

> Stub — mana pool, regeneration, costs, and HUD display.

## Design

TBD — see game concept **Mana** in [CONTEXT.md](../../CONTEXT.md).

## Implementation

| Concern | Location |
|---------|----------|
| Script values | `common/script_values/01_ancient_magic_mana_system_values.txt` |
| Monthly pulse | `common/on_action/01_ancient_magic_mana_system_on_actions.txt` |
| Spell mana effects | `common/scripted_effects/01_ancient_magic_spell_system_effects.txt` |
| HUD | `gui/custom_gui/mana_system_hud.gui` |

## Related docs

- [spellbook-and-auras.md](spellbook-and-auras.md)
- [advancement.md](advancement.md)
- [mana-affinity-interactions.md](../event-chains/mana-affinity-interactions.md)
- [spell-index.md](../systems/spell-index.md)
- [gui-and-hud.md](../systems/gui-and-hud.md)
