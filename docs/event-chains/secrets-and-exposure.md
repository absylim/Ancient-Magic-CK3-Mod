---
status: stub
audience: dev-design
code_roots:
  - events/ancient_magic_secret_events.txt
  - common/secret_types/00_magic_secrets.txt
  - common/scripted_effects/00_ancient_magic_secret_effects.txt
  - common/scripted_triggers/01_ancient_magic_secret_type_trigger.txt
loc_roots:
  - localization/english/event_localization/ancient_magic_secret_events_l_english.yml
trello:
---

# Secrets and Exposure

> Stub — magic secret discovery, exposure, and consequences.

## Entry points

| Trigger | Location |
|---------|----------|
| Secret events | `events/ancient_magic_secret_events.txt` (`magic_secrets`) |
| Secret types | `common/secret_types/00_magic_secrets.txt` |

## Flow

```mermaid
flowchart TD
  TBD[TBD]
```

## Event ID table

| Event ID | Purpose | Loc keys |
|----------|---------|----------|
| `magic_secrets.*` | TBD | TBD |

## Known gaps

- Full exposure graph TBD

## Related docs

- [mechanics/traits-secrets-and-potential.md](../mechanics/traits-secrets-and-potential.md)
