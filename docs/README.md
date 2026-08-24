# Ancient Magic — Developer Documentation

Internal mod design and development reference for contributors. This is **not** player-facing Steam page copy.

## Doc map

| Folder | Audience | Code roots | Primary localization |
|--------|----------|------------|----------------------|
| [mechanics/](mechanics/) | Designers + devs | `common/script_values/`, `common/traits/`, perk trees | `localization/english/ancient_magic_game_concepts_l_english.yml` |
| [schools/](schools/) | Designers + devs | `common/lifestyle_perks/`, school scripted GUIs | `localization/english/ancient_magic_spells_l_english.yml` (per school) |
| [event-chains/](event-chains/) | Designers + devs | `events/` | Event localization under `localization/english/` |
| [features/](features/) | Designers + devs | `common/buildings/`, `common/artifacts/`, mapmode scripts | Feature-specific loc files |
| [systems/](systems/) | Developers | `common/on_action/`, `common/scripted_*`, `gui/` | Mixed — see each index |
| [adr/](adr/) | Maintainers | N/A (decision records only) | N/A |
| [CONTEXT.md](../CONTEXT.md) | Everyone | Glossary only — **no file paths** | `localization/english/ancient_magic_game_concepts_l_english.yml` |

## Source-of-truth order

When docs disagree with other artifacts, resolve in this order:

1. **Code** (`common/`, `events/`, `gui/`) — implementation truth
2. **Localization game concepts** (`ancient_magic_game_concepts_l_english.yml`) — player-facing term definitions
3. **In-repo docs** (`docs/`, `CONTEXT.md`) — settled design intent
4. **Trello / Discord** — roadmap, WIP, and discussion (not canonical until written in-repo)

## External links

- **Trello:** https://trello.com/b/Z54JlSqO/ancient-magic
- **Discord:** https://discord.gg/jTSaFKm
- **Steam:** https://steamcommunity.com/sharedfiles/filedetails/?id=2224273167

## How to write a doc

1. Pick the template for your folder (`_template.md` in `schools/`, `event-chains/`, `features/`, or `systems/`; `mechanics/` uses the feature template shape — see [features/_template.md](features/_template.md)).
2. Copy the template to a new file; set frontmatter `status: draft` when you start filling content.
3. Fill **Design** first (player experience, balance levers); put wiring in **Implementation** or a `systems/` doc.
4. Link code paths with repo-root-relative paths (`common/...`, `events/...`) — do not invent script names; mark unknowns `TBD`.
5. When a term settles, add it to [CONTEXT.md](../CONTEXT.md) (definitions only — no implementation detail).
6. Optional: add a `trello:` URL in frontmatter when a Trello card tracks WIP for that page.

## Vanilla override convention

Event-chain docs **must** flag when the mod overrides vanilla namespaces or event files. Use a prominent callout block at the top of the doc. Reference pattern: [event-chains/education-and-childhood.md](event-chains/education-and-childhood.md).

## Status lifecycle

`stub` → `draft` → `review` → `verified`

- **stub** — navigation anchor only; structure and code_roots pre-filled
- **draft** — partial content; may have `TBD` sections
- **review** — content complete; awaiting maintainer review
- **verified** — matches current code and loc

## Maintenance triggers

Update docs when any of these change:

- Event IDs or namespaces (`events/`)
- Decision keys (`common/decisions/`)
- Perk tree keys or perk IDs (`common/lifestyle_perks/`)
- Game rules (`common/game_rules/`)
- On-action hooks that drive pulses (`common/on_action/`)

## Future population workflow

Recommended fill order (not part of v1 scaffold):

1. Core loop — mana → potential → mage trait → spellbook (`mechanics/`)
2. Event chains with decisions — rituals, religious doctrine (`event-chains/`)
3. Chiromancy pilot — health-and-healing + chiromancy school + spell-index seed row
4. Systems indexes — on_actions, decisions, bitmask
5. Remaining schools + features

## Conventions

1. **Do not invent script names** — every trigger/effect/on_action referenced must exist in code or be marked `TBD`.
2. **Link, don't copy** — spell lists point to [systems/spell-index.md](systems/spell-index.md) and loc keys; avoid duplicating 100+ spells by hand.
3. **Design vs implementation** — `mechanics/` and `features/` lead with `## Design`; dev wiring lives in `systems/` or `## Implementation`.
4. **Event chain diagrams** — mermaid in event-chain docs; IDs must match `namespace.event_id` in code.
5. **Vanilla overrides** — event-chain docs flag overridden vanilla namespaces; education stub is the exemplar.
6. **Glossary sync** — settled terms go in root `CONTEXT.md`; never file paths or implementation in CONTEXT.
7. **Trello for WIP** — optional `trello:` frontmatter; settled design lives in-repo.
8. **ADRs are rare** — only irreversible cross-cutting decisions in `docs/adr/`.
9. **No game-load impact** — `docs/` is outside CK3 load paths; no `descriptor.mod` changes needed.
