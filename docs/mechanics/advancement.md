---
status: stub
audience: dev-design
code_roots:
  - common/scripted_effects/01_ancient_magic_advancement_effects.txt
  - common/on_action/00_ancient_magic_magic_advancement_on_actions.txt
  - common/script_values/01_ancient_magic_advancement_values.txt
  - gui/window_magic_advancement.gui
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
  - localization/english/tutorial/ancient_magic_tutorial_l_english.yml
trello:
---

# Advancement (Magical Studies)

> Stub — **design only**. Bitmask implementation details live in [systems/bitmask-and-unlocks.md](../systems/bitmask-and-unlocks.md).

## Design

TBD — how characters progress from magic potential to active mages; mage level growth.

## Implementation

Summary only — see systems doc for bitmask wiring.

| Concern | Location |
|---------|----------|
| Advancement effects | `common/scripted_effects/01_ancient_magic_advancement_effects.txt` |
| Pulses | `common/on_action/00_ancient_magic_magic_advancement_on_actions.txt` |
| Values | `common/script_values/01_ancient_magic_advancement_values.txt` |
| UI | `gui/window_magic_advancement.gui` |

## Related docs

- [systems/bitmask-and-unlocks.md](../systems/bitmask-and-unlocks.md)
- [traits-secrets-and-potential.md](traits-secrets-and-potential.md)
- [schools/general-magic.md](../schools/general-magic.md)
