---
status: stub
audience: dev-design
code_roots:
  - events/ancient_magic_debug_events.txt
  - common/decisions/ancient_magic_debug_decisions.txt
loc_roots:
  - localization/english/event_localization/ancient_magic_debug_events_l_english.yml
trello:
---

# Debug and Devtools

> Stub — maintainer-facing debug events and decisions.

## Overview

Tools for testing spells, advancement, and magic systems in dev builds. Not player documentation.

## Components

| File | Role |
|------|------|
| `events/ancient_magic_debug_events.txt` | Debug event namespace |
| `common/decisions/ancient_magic_debug_decisions.txt` | Debug decisions menu |

## Failure modes

- Debug content must not ship enabled in production builds without review (TBD — game rule or compile flag)

## Related docs

- [systems/decisions-index.md](decisions-index.md)
