---
status: stub
audience: dev-design
code_roots:
  - common/on_action/01_ancient_magic_spellbook_on_actions.txt
  - common/scripted_effects/01_ancient_magic_spellbook_effects.txt
  - common/scripted_effects/01_ancient_magic_spell_effects.txt
  - common/scripted_guis/magic_scripted_gui.txt
  - gui/window_magic.gui
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
trello:
---

# Spellbook and Auras

> Stub — spellbook UI, casting, auras vs one-shot spells.

## Design

TBD — distinguish **Spell**, **Aura**, and **Ritual** per [CONTEXT.md](../../CONTEXT.md).

## Implementation

| Concern | Location |
|---------|----------|
| Spellbook pulse | `common/on_action/01_ancient_magic_spellbook_on_actions.txt` |
| Spellbook effects | `common/scripted_effects/01_ancient_magic_spellbook_effects.txt` |
| Per-spell effects | `common/scripted_effects/01_ancient_magic_spell_effects.txt` |
| Scripted GUI | `common/scripted_guis/magic_scripted_gui.txt` (+ school variants) |
| Window | `gui/window_magic.gui` |

## Related docs

- [systems/spell-index.md](../systems/spell-index.md)
- [systems/gui-and-hud.md](../systems/gui-and-hud.md)
- [mana-system.md](mana-system.md)
