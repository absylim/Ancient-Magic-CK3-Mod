# CK3 mod file structure

CK3 is data-driven: plaintext `.txt` scripts, YAML localization, `.gui` UI, and
`gfx/` assets. Mods mirror the game's top-level folders.

## Top-level directories

| Folder | Purpose |
|--------|---------|
| `common/` | Game rules, definitions, scripted logic |
| `events/` | Event definitions and chains |
| `gfx/` | Textures, icons, models, portraits |
| `gui/` | Interface layouts and widgets |
| `history/` | Characters, titles, provinces at start dates |
| `localization/` | In-game text by language |

## File naming and load order

Files load in **alphabetical order**. Later files override earlier ones with the
same block key.

**Pattern:** `PREFIX_NUMBER_descriptive_name.txt`

- **PREFIX:** 2–5 unique letters identifying your mod (e.g. `mymod`, `ww`)
- **NUMBER:** Zero-padded load order; lower numbers load first
- **descriptive_name:** What the file contains

Example: `mymod_01_grand_tournament_decision.txt`

Use fully unique filenames. A name collision with vanilla or another mod causes
silent overwrites based on mod load order.

## common/ subfolders (by domain)

### Scripting primitives

| Folder | Content |
|--------|---------|
| `scripted_triggers/` | Reusable boolean conditions |
| `scripted_effects/` | Reusable effect blocks |
| `script_values/` | Named numeric formulas |
| `scripted_modifiers/` | Dynamic modifier calculations |
| `scripted_lists/` | Named scope lists |
| `on_action/` | Engine lifecycle hooks |
| `defines/` | Engine constant overrides |

### Character and interaction

`character_interactions/`, `character_interaction_categories/`,
`character_backgrounds/`, `character_memory_types/`, `courtier_guest_management/`,
`guest_system/`, `pool_character_selectors/`, `nicknames/`, `deathreasons/`,
`secret_types/`, `hook_types/`, `important_actions/`

### Government and realm

`governments/`, `laws/`, `council_positions/`, `council_tasks/`,
`court_types/`, `court_positions/`, `court_amenities/`, `decisions/`,
`decision_group_types/`, `subject_contracts/`, `lease_contracts/`,
`vassal_stances/`, `diarchies/`, `domiciles/`, `legitimacy/`, `tax_slots/`

### Titles and succession

`landed_titles/`, `succession_appointment/`, `succession_election/`

### Dynasty and houses

`dynasties/`, `dynasty_houses/`, `dynasty_legacies/`, `dynasty_perks/`,
`house_aspirations/`, `house_relation_types/`, `house_unities/`

### Culture and religion

`culture/`, `religion/`, `ethnicities/`

### Military and warfare

`men_at_arms_types/`, `casus_belli_types/`, `casus_belli_groups/`,
`combat_effects/`, `combat_phase_events/`, `raids/`, `ai_war_stances/`

### Modifiers and opinions

`modifiers/`, `opinion_modifiers/`, `modifier_definition_formats/`,
`modifier_icons/`

### Intrigue and lifestyle

`schemes/`, `factions/`, `lifestyles/`, `focuses/`, `lifestyle_perks/`

### Activities and narrative

`activities/`, `travel/`, `story_cycles/`, `situation/`, `struggle/`,
`great_projects/`

### Artifacts and legends

`artifacts/`, `legends/`, `inspirations/`

### Localization helpers

`customizable_localization/`, `trigger_localization/`, `effect_localization/`,
`flavorization/`, `messages/`

### Visual and UI data

`coat_of_arms/`, `genes/`, `dna_data/`, `bookmark_portraits/`, `bookmarks/`,
`named_colors/`, `event_backgrounds/`, `event_themes/`, `event_transitions/`,
`portrait_types/`, `game_concepts/`

### Traits

`traits/` — character trait definitions

## events/ structure

Events use namespaces and numeric IDs: `namespace.0001`.

```
events/
├── activities/
├── court_events/
├── decisions_events/
├── scheme_events/
├── war_events/
├── yearly_events/
├── birth_events.txt
├── death_events.txt
└── ...
```

### Minimal event block

```pdx
namespace = mymod

mymod.0001 = {
    type = character_event
    title = mymod.0001.t
    desc = mymod.0001.desc
    theme = default

    trigger = { is_ruler = yes }

    immediate = { }

    option = {
        name = mymod.0001.a
        add_prestige = 50
    }
}
```

### Event types

`character_event`, `letter_event`, `court_event`, `activity_event` — default is
`character_event`.

### Key optional fields

| Field | Purpose |
|-------|---------|
| `scope` | Override expected root scope |
| `window` | Custom event window GUI |
| `cooldown` | Prevents re-fire for duration |
| `left_portrait` / `right_portrait` | Character portraits |
| `widgets` | Embedded custom GUI widgets |
| `on_trigger_fail` | Effect when queued event fails trigger |
| `orphan = yes` | Suppress unreferenced-event warnings |

## gfx/ structure

| Subfolder | Content |
|-----------|---------|
| `interface/` | UI icons (traits, modifiers, etc.) |
| `portraits/` | Portrait assets |
| `coat_of_arms/` | CoA textures |
| `map/` | Map textures |
| `models/` | 3D models |
| `court_scene/` | Royal Court 3D assets |

Trait icons default path: `gfx/interface/icons/traits/<trait_key>.dds`

## gui/ structure

| Area | Examples |
|------|----------|
| Core HUD | `hud.gui`, `hud_top.gui`, `hud_bottom.gui` |
| Event windows | `event_windows/`, `event_window_widgets/` |
| Gameplay windows | `window_decisions.gui`, `window_intrigue.gui` |
| Activity widgets | `activity_window_widgets/` |
| Shared components | `shared/`, `scripted_widgets/` |

GUI files use Paradox's custom layout language (`.gui`).

## history/ structure

```
history/
├── characters/
├── titles/
├── provinces/
├── cultures/
├── artifacts/
└── wars/
```

Used for bookmark starts and historical setup. Follow `_characters.info`,
`_history.info`, `_provinces.info` format guides when present in vanilla.

## localization/ structure

```
localization/
├── english/
├── french/
├── german/
└── ...
```

Each language folder contains `.yml` files. See `localization.md` for format.

## Version notes (v18 → v19)

- `vassal_contracts/` renamed to `subject_contracts/`
- `struggles/` renamed to `struggle/`
- v19 adds: `house_aspirations/`, `story_cycles/`, `situation/`,
  `great_projects/`, `succession_appointment/`, `succession_election/`,
  expanded `activities/`
- Religion file layout changed; verify faith/doctrine paths before editing

## Modding workflow

1. Plan which systems to modify
2. Create uniquely prefixed files in correct folders
3. Write script blocks with verified triggers/effects
4. Add localization for all visible text
5. Add gfx/gui assets for custom visuals
6. Test in-game; read `error.log` for script failures
