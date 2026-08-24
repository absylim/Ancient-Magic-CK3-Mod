---
status: stub
audience: dev-design
code_roots:
  - common/scripted_triggers/ancient_magic_ley_lines_triggers.txt
  - common/scripted_effects/00_ancient_magic_map_effects.txt
  - common/scripted_guis/ancient_magic_mapmodes_gui.txt
  - gui/shared/mapmodes.gui
  - gui/shared/magic_lines.gui
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
trello:
---

# Ley Lines and Mapmode

> Stub — ley lines, focal points, map population, and mapmode UI.

## Design

TBD — see **Ley Line** and **Focal Point** in [CONTEXT.md](../../CONTEXT.md).

## Implementation

| Concern | Location |
|---------|----------|
| Ley line triggers | `common/scripted_triggers/ancient_magic_ley_lines_triggers.txt` |
| Map effects | `common/scripted_effects/00_ancient_magic_map_effects.txt` |
| Mapmode GUI | `common/scripted_guis/ancient_magic_mapmodes_gui.txt` |
| Map GUI | `gui/shared/mapmodes.gui`, `gui/shared/magic_lines.gui` |

## Related docs

- [event-chains/game-start-setup.md](../event-chains/game-start-setup.md)
