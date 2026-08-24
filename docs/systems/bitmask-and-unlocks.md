---
status: stub
audience: dev-design
code_roots:
  - common/scripted_effects/01_ancient_magic_bitmask_effects.txt
  - common/scripted_triggers/01_ancient_magic_bitmask_triggers.txt
  - common/script_values/01_ancient_magic_bitmask_values.txt
  - common/trigger_localization/01_ancient_magic_advancement_trigger_localization.txt
loc_roots:
  - localization/english/trigger_localization/ancient_magic_trigger_localization_l_english.yml
trello:
---

# Bitmask and Unlocks

> Stub — **implementation** of advancement unlocks via bitmasks. Player-facing design is in [mechanics/advancement.md](../mechanics/advancement.md).

## Overview

How perk/spell unlock state is stored and checked (bitmask variables, triggers, effects).

## Components

| File | Role |
|------|------|
| `common/scripted_effects/01_ancient_magic_bitmask_effects.txt` | Set/clear unlock bits |
| `common/scripted_triggers/01_ancient_magic_bitmask_triggers.txt` | Unlock checks |
| `common/script_values/01_ancient_magic_bitmask_values.txt` | Bitmask math |

## Key variables

TBD — variable names and bit layout during population pass.

## Failure modes

- TBD — desync between GUI display and bitmask state

## Related docs

- [mechanics/advancement.md](../mechanics/advancement.md)
- [adr/](../adr/) — candidate ADR topic if bitmask approach is documented as irreversible
