---
name: Mod docs scaffold
overview: "Create a developer/designer-focused `docs/` scaffold for Ancient Magic CK3 Mod: folder layout mirroring existing code domains, navigation index, reusable templates, root CONTEXT.md glossary shell, and ~30 stubs—no populated mechanic content yet. Decisions settled via grilling session."
todos:
  - id: create-docs-tree
    content: "Create docs/ folder tree: mechanics/, schools/, event-chains/, features/, systems/, adr/"
    status: pending
  - id: write-readme-hub
    content: Write docs/README.md with navigation, conventions, Trello/Discord links, vanilla-override rule, source-of-truth order
    status: pending
  - id: create-templates
    content: Add _template.md for schools, event-chains, features, and systems (4 templates)
    status: pending
  - id: create-stub-pages
    content: Create all stub .md files (~30) with frontmatter, pre-filled code entry points, cross-links
    status: pending
  - id: create-context-glossary
    content: Create root CONTEXT.md glossary shell seeded from game_concepts localization
    status: pending
  - id: update-root-readme
    content: Add one-line pointer from README.md to docs/README.md
    status: pending
isProject: false
---

# Ancient Magic Mod Documentation Scaffold

## Goal

Establish a durable documentation home in the mod repo so future work can target specific mechanics, event chains, and features without re-discovering the codebase. First pass is **scaffold only**: folders, index, templates, glossary shell, and full stub set—aligned to what already exists under [`common/`](common/) and [`events/`](events/).

**Audience:** mod developers and designers (design intent, flows, balance notes, code cross-refs).  
**Out of scope for v1:** player-facing guides, full content population, automated doc generation.

---

## Settled decisions (grilling session)

| Decision | Choice |
|----------|--------|
| Glossary location | [`CONTEXT.md`](CONTEXT.md) at **repo root**; `docs/README.md` links to it |
| Authority order | **Hybrid:** settled mechanics/design in-repo; roadmap/WIP on Trello; code is implementation truth |
| Trello wiring | `docs/README.md` links section + optional `trello:` URL in stub frontmatter |
| `mechanics/` vs `systems/` | **mechanics/** = designer-facing player rules; **systems/** = dev architecture (pulses, bitmasks, indexes) |
| Stub count | **Full scaffold** (~30 stubs)—empty pages as navigation anchors |
| Debug tooling | **systems/debug-and-devtools.md** stub for maintainers |
| Bitmask docs | **Split:** `mechanics/advancement.md` (design) + `systems/bitmask-and-unlocks.md` (implementation) |
| AI spellcasting | **No** `features/ai-spellcasting.md`; each **school stub** gets an AI section |
| AI healing events | Folded into **event-chains/health-and-healing.md** (not a separate AI events stub) |
| Spell reference (future) | **systems/spell-index.md** as master cross-school table; school docs link to it |
| Religion docs | **Split:** `features/religion-integration.md` (doctrine system) + `event-chains/religious-doctrine.md` (HoF/convocation flows) |
| Game-start setup | **Primary** in `event-chains/game-start-setup.md`; cross-link from `mechanics/traits-secrets-and-potential.md` |
| Vanilla overrides | **Convention** in `docs/README.md`; **education-and-childhood.md** is the reference exemplar stub |
| School naming | **general-magic.md** (perk tree key `ancient_magic_basic`) |
| Frontmatter | `status`, `audience`, `code_roots`, `loc_roots`, `trello` (optional) |
| Status lifecycle | `stub` → `draft` → `review` → `verified` |
| ADR threshold | **Rare**—only irreversible cross-cutting choices (e.g. bitmask vs trait flags) |
| Link style | **Repo-root-relative** for all links (`docs/schools/...`, `common/...`) |
| Templates | **Four:** schools, event-chains, features, systems (dedicated systems template) |

---

## Proposed folder layout (revised)

```text
docs/
├── README.md
├── adr/
│   └── README.md                      # Rare ADR usage; irreversible cross-cutting only
├── mechanics/                         # Designer-facing player rules
│   ├── README.md
│   ├── _template.md                   # reuses feature template shape
│   ├── mana-system.md
│   ├── spellbook-and-auras.md
│   ├── advancement.md                 # was advancement-and-bitmasks; design only
│   ├── traits-secrets-and-potential.md  # cross-links game-start-setup
│   └── game-rules.md
├── schools/                           # Per school: perks, spells (link to spell-index), AI section
│   ├── README.md
│   ├── _template.md
│   ├── general-magic.md
│   ├── chronomancy.md
│   ├── chiromancy.md
│   ├── elomancy.md
│   ├── necromancy.md
│   ├── biomancy.md
│   └── mensomancy.md
├── event-chains/                      # Flows + mermaid; must flag vanilla overrides
│   ├── README.md
│   ├── _template.md
│   ├── rituals-and-blood.md
│   ├── health-and-healing.md          # AMHealth + AI spellbook events .301–.303
│   ├── religious-doctrine.md          # HoF/convocation flows only
│   ├── education-and-childhood.md     # VANILLA OVERRIDE exemplar stub
│   ├── secrets-and-exposure.md
│   ├── mana-affinity-interactions.md
│   └── game-start-setup.md            # cross-links traits-secrets-and-potential
├── features/                          # Cross-cutting world/map systems
│   ├── README.md
│   ├── _template.md
│   ├── ley-lines-and-mapmode.md
│   ├── buildings-and-city-of-magic.md
│   ├── artifacts-and-ingredients.md
│   ├── religion-integration.md        # doctrine system + faith patches
│   ├── dynasty-legacy.md
│   └── tutorial.md
└── systems/                           # Dev architecture + indexes
    ├── README.md
    ├── _template.md
    ├── on-actions-index.md
    ├── decisions-index.md
    ├── scripted-logic-index.md
    ├── gui-and-hud.md
    ├── bitmask-and-unlocks.md
    ├── spell-index.md                 # future master spell table
    └── debug-and-devtools.md
```

**Removed from original plan:** `features/ai-spellcasting.md`, `mechanics/advancement-and-bitmasks.md`, `event-chains/magical-health.md`

**CONTEXT.md** lives at repo root (not under `docs/`).

---

## Key files to create (content shape)

### 1. [`docs/README.md`](docs/README.md) — navigation hub

Sections:
- **Purpose** — internal mod design/dev reference (not Steam page copy)
- **Doc map** — table linking each folder to code roots and primary localization
- **Source-of-truth order** — code > localization game concepts > in-repo docs > Trello/Discord (WIP/roadmap)
- **External links** — Trello + Discord (canonical URLs from root README)
- **How to write a doc** — pick template, fill sections, link code paths, update `CONTEXT.md` when terms settle
- **Vanilla override convention** — event-chain docs must flag when mod overrides vanilla namespaces/files; see `education-and-childhood.md` as exemplar
- **Status lifecycle** — stub → draft → review → verified
- **Maintenance triggers** — update when event IDs, decisions, or perk trees change

### 2. [`CONTEXT.md`](CONTEXT.md) — glossary shell (repo root)

Per [CONTEXT-FORMAT.md](.agents/skills/domain-modeling/CONTEXT-FORMAT.md), seed **canonical terms only** (no implementation detail, no file paths):

| Term | Seed definition source |
|------|------------------------|
| Mana | `localization/english/ancient_magic_game_concepts_l_english.yml` |
| Mana Affinity | same |
| Magic Potential | same |
| Mage Level | same |
| School of Magic | same (+ six school names) |
| Mage Ritual | same |
| Magical Bloodline | same |
| Ley Line / Focal Point | game concepts loc |
| City of Magic | game concepts loc |

Each entry: 1–2 sentence definition + `_Avoid_` aliases where the mod uses inconsistent names.

### 3. Templates (four)

**School** (`docs/schools/_template.md`): design intent, perk tree key, spells (link to `systems/spell-index.md`), **AI behavior section**, related events/decisions.

**Event chain** (`docs/event-chains/_template.md`): entry points, mermaid flowchart, event ID table, loc keys, **vanilla override callout** (if applicable), known gaps.

**Feature / mechanic** (`docs/features/_template.md`, reused by `mechanics/`): Design + Implementation sections, player experience, variables/traits, code entry points, dependencies, balance levers.

**Systems** (`docs/systems/_template.md`): components, pulse/hook graph, key variables, file index table, failure modes.

### 4. Stub frontmatter schema

```yaml
---
status: stub          # stub | draft | review | verified
audience: dev-design
code_roots:
  - common/...
  - events/...
loc_roots:
  - localization/english/...
trello:               # optional — link when Trello card exists
---
```

All links use **repo-root-relative** paths (`common/...`, `docs/schools/...`).

### 5. Special stub notes

- **education-and-childhood.md** — includes prominent "Vanilla override" callout as reference pattern for other event-chain docs
- **health-and-healing.md** — covers `AMHealth` events AND `ancient_magic_ai_spellbook_events.301`–`.303`; notes missing `.001` back-menu event
- **traits-secrets-and-potential.md** — cross-links `event-chains/game-start-setup.md`
- **religion-integration.md** vs **religious-doctrine.md** — feature doc owns doctrine patches; event-chain doc owns HoF/convocation narrative flows only

---

## Conventions (embedded in docs/README.md)

1. **Do not invent script names** — every trigger/effect/on_action referenced must exist in code or be marked `TBD`.
2. **Link, don't copy** — spell lists point to `systems/spell-index.md` and loc keys; avoid duplicating 100+ spells by hand.
3. **Design vs implementation** — `mechanics/` and `features/` lead with `## Design`; dev wiring lives in `systems/` or `## Implementation`.
4. **Event chain diagrams** — mermaid in event-chain docs; IDs must match `namespace.event_id` in code.
5. **Vanilla overrides** — event-chain docs flag overridden vanilla namespaces; education stub is the exemplar.
6. **Glossary sync** — settled terms go in root `CONTEXT.md`; never file paths or implementation in CONTEXT.
7. **Trello for WIP** — optional `trello:` frontmatter; settled design lives in-repo.
8. **ADRs are rare** — only irreversible cross-cutting decisions in `docs/adr/`.
9. **No game-load impact** — `docs/` outside CK3 load paths; no `descriptor.mod` changes.

---

## Optional small README touch

Add one line to [`README.md`](README.md) pointing to `docs/README.md` for contributor documentation.

---

## Future population workflow (documented in docs/README, not v1)

1. Core loop — mana → potential → mage trait → spellbook (`mechanics/`)
2. Event chains with decisions — rituals, religious doctrine (`event-chains/`)
3. Chiromancy pilot — health-and-healing + chiromancy school + spell-index seed row
4. Systems indexes — on_actions, decisions, bitmask
5. Remaining schools + features

---

## Verification

After scaffold creation:
- All stub files exist (~30) and render valid markdown
- Every stub's `code_roots` paths resolve to real files in the repo
- `docs/README.md` links resolve locally
- `CONTEXT.md` at repo root contains only glossary terms
- `education-and-childhood.md` includes vanilla override exemplar callout
- No files added under `common/`, `events/`, or `localization/`
