---
status: stub
audience: dev-design
code_roots:
  - events/ancient_magic_religious_events.txt
  - common/decisions/01_magic_religious_decisions.txt
  - common/scripted_effects/01_ancient_magic_religion_effects.txt
loc_roots:
  - localization/english/event_localization/ancient_magic_religious_events_l_english.yml
trello:
---

# Religious Doctrine (HoF / Convocation)

> Stub — **narrative flows only** (Head of Faith challenges, convocation). Doctrine patches live in [features/religion-integration.md](../features/religion-integration.md).

## Entry points

| Trigger | Location |
|---------|----------|
| Religious events | `events/ancient_magic_religious_events.txt` (`am_religious_special`) |
| Religious decisions | `common/decisions/01_magic_religious_decisions.txt` |

## Flow

```mermaid
flowchart TD
  TBD[TBD]
```

## Event ID table

| Event ID | Purpose | Loc keys |
|----------|---------|----------|
| `am_religious_special.1001` | HoF challenge — preparation | TBD |
| TBD | Convocation flows | TBD |

## Known gaps

- Full event catalog TBD

## Related docs

- [features/religion-integration.md](../features/religion-integration.md)
