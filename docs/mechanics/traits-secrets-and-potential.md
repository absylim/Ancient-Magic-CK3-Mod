---
status: stub
audience: dev-design
code_roots:
  - common/traits/01_ancient_magic_traits.txt
  - common/secret_types/00_magic_secrets.txt
  - common/scripted_effects/00_ancient_magic_secret_effects.txt
  - events/ancient_magic_secret_events.txt
  - events/ancient_magic_gain_talent_events.txt
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
trello:
---

# Traits, Secrets, and Magic Potential

> Stub — mage trait, magic secrets, mana affinity, and magic potential traits.

## Design

TBD — relationship between **Mana Affinity**, **Magic Potential**, mage trait, and secrets.

## Implementation

| Concern | Location |
|---------|----------|
| Traits | `common/traits/01_ancient_magic_traits.txt` |
| Secret types | `common/secret_types/00_magic_secrets.txt` |
| Secret effects | `common/scripted_effects/00_ancient_magic_secret_effects.txt` |
| Secret events | `events/ancient_magic_secret_events.txt` (`magic_secrets`) |
| Talent / potential events | `events/ancient_magic_gain_talent_events.txt` |

## Related docs

- [event-chains/game-start-setup.md](../event-chains/game-start-setup.md) — primary game-start wiring
- [event-chains/secrets-and-exposure.md](../event-chains/secrets-and-exposure.md)
- [CONTEXT.md](../../CONTEXT.md)
