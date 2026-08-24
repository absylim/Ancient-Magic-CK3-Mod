---
status: stub
audience: dev-design
code_roots:
  - common/scripted_effects/
  - common/scripted_triggers/
  - common/script_values/
  - common/scripted_modifiers/
  - common/scripted_guis/
loc_roots:
  - localization/english/effect_localization/
  - localization/english/trigger_localization/
trello:
---

# Scripted Logic Index

> Stub — map of scripted effects, triggers, values, modifiers, and GUIs.

## Overview

Entry point for developers tracing behavior through `common/scripted_*` and related localization.

## Components

| Folder | Purpose |
|--------|---------|
| `common/scripted_effects/` | Reusable effect blocks (spells, perks, health, religion, …) |
| `common/scripted_triggers/` | Conditions (bitmask, ley lines, health, …) |
| `common/script_values/` | Numeric formulas (mana, advancement, spell cost, …) |
| `common/scripted_modifiers/` | Character modifiers |
| `common/scripted_guis/` | GUI logic and compatibility layers |

## File index

TBD — grouped tables by domain during population pass.

## Related docs

- [systems/bitmask-and-unlocks.md](bitmask-and-unlocks.md)
- [systems/gui-and-hud.md](gui-and-hud.md)
