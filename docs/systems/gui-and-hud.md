---
status: stub
audience: dev-design
code_roots:
  - gui/window_magic.gui
  - gui/window_magic_advancement.gui
  - gui/hud.gui
  - gui/hud_bottom.gui
  - gui/custom_gui/mana_system_hud.gui
  - common/scripted_guis/
loc_roots:
  - localization/english/magic_gui_l_english.yml
trello:
---

# GUI and HUD

> Stub — magic windows, HUD widgets, and scripted GUI wiring.

## Overview

Player-facing UI: spellbook window, advancement window, mana HUD, trait initializer, and school-specific scripted GUIs.

## Components

| File | Role |
|------|------|
| `gui/window_magic.gui` | Main spellbook window |
| `gui/window_magic_advancement.gui` | Magical studies / advancement |
| `gui/custom_gui/mana_system_hud.gui` | Mana HUD |
| `gui/hud.gui`, `gui/hud_bottom.gui` | HUD integration |
| `common/scripted_guis/magic_scripted_gui*.txt` | Per-school GUI logic |

## Failure modes

- TBD — GUI compatibility triggers in `magic_gui_compatibility_*`

## Related docs

- [mechanics/spellbook-and-auras.md](../mechanics/spellbook-and-auras.md)
- [mechanics/mana-system.md](../mechanics/mana-system.md)
