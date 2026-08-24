---
status: stub
audience: dev-design
code_roots:
  - common/on_action/
loc_roots:
  - localization/english/
trello:
---

# On Actions Index

> Stub — catalog of `common/on_action/` hooks driving Ancient Magic pulses.

## Overview

Central index for game-start, monthly, yearly, and domain-specific on_actions.

## Components

| File | Role |
|------|------|
| `common/on_action/01_ancient_magic_game_start_actions.txt` | Post-lobby setup |
| `common/on_action/01_ancient_magic_mana_system_on_actions.txt` | Mana monthly pulse |
| `common/on_action/01_ancient_magic_spellbook_on_actions.txt` | Spellbook maintenance |
| `common/on_action/00_ancient_magic_magic_advancement_on_actions.txt` | Advancement pulses |
| `common/on_action/01_ancient_magic_health_on_actions.txt` | Health system |
| `common/on_action/01_ancient_magic_childhood_on_actions.txt` | Childhood / education |
| `common/on_action/01_ancient_magic_child_birth_on_actions.txt` | Birth hooks |
| `common/on_action/01_ancient_magic_title_on_actions.txt` | Title changes |
| `common/on_action/00_ancient_magic_immortal_on_actions.txt` | Immortal trait |
| `common/on_action/magic_on_actions.txt` | Base `on_game_start` |

## Pulse / hook graph

TBD — document dependency order during population pass.

## Related docs

- [event-chains/game-start-setup.md](../event-chains/game-start-setup.md)
