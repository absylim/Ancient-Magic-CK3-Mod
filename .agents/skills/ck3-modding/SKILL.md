---
name: ck3-modding
description: >-
  Guides Crusader Kings 3 (CK3) mod development: PDX script syntax, scopes,
  triggers, effects, events, decisions, traits, culture, faith, government,
  laws, localization, GUI, and debugging. Use when modding CK3, writing .txt
  game scripts, creating events or decisions, adding traits or modifiers,
  working with on_action hooks, or analyzing Paradox CK3 data files.
---

# CK3 modding

Self-contained skill for Crusader Kings 3 mod scripting and content authoring.
Read reference files in `reference/` for topic depth; do not depend on external
repo docs when this skill is deployed elsewhere.

## When to use

Apply this skill when the user:

- Creates or edits CK3 mod files (`.txt`, `.gui`, localization `.yml`)
- Asks about triggers, effects, scopes, script values, or on_actions
- Builds events, decisions, traits, cultures, faiths, laws, or schemes
- Debugs mod load errors, missing localization, or script failures
- Wants mod folder structure, naming conventions, or load order rules

## Read order

1. [reference/mod-structure.md](reference/mod-structure.md) — folders, naming, load order
2. [reference/scripting-primitives.md](reference/scripting-primitives.md) — syntax, scopes, triggers, effects
3. Domain reference matching the task (table below)
4. [examples.md](examples.md) — end-to-end patterns

## Domain quick map

| Task | Reference |
|------|-----------|
| Events, decisions, modifiers | [reference/events-and-decisions.md](reference/events-and-decisions.md) |
| on_action hooks, script values | [reference/on-actions-and-values.md](reference/on-actions-and-values.md) |
| Localization, tooltips | [reference/localization.md](reference/localization.md) |
| Traits | [reference/traits.md](reference/traits.md) |
| Culture, faith, religion | [reference/culture-and-faith.md](reference/culture-and-faith.md) |
| Government, laws, warfare, succession | [reference/realm-systems.md](reference/realm-systems.md) |
| Character interactions, court | [reference/character-and-court.md](reference/character-and-court.md) |
| Schemes, intrigue, factions | [reference/schemes-and-intrigue.md](reference/schemes-and-intrigue.md) |
| GUI, gfx, portraits | [reference/ui-and-graphics.md](reference/ui-and-graphics.md) |
| Debugging, validation | [reference/debugging.md](reference/debugging.md) |

## Core workflows

### 1. Find vanilla examples

When a game install or extracted files are available:

1. Identify the system folder (`common/decisions/`, `events/`, etc.)
2. Search for a similar vanilla definition by keyword or mechanic name
3. Copy structure, rename with your mod prefix, adapt keys
4. Never invent trigger/effect/on_action names — verify against vanilla or engine docs

In **CK3-AI-IDE-Docs**, prefer `Code/CK_Ver_19/` as the authoritative baseline.
Use `Code/CK_Ver_18/` only for version-diff context.

### 2. Create a new mod file

```
Task progress:
- [ ] Choose PREFIX (2–5 unique letters) and NUMBER for load order
- [ ] Place file in correct folder (see mod-structure reference)
- [ ] Define block with unique key matching filename prefix
- [ ] Add localization keys in localization/english/
- [ ] Add gfx assets if UI/portrait references exist
- [ ] Test in-game; check error.log
```

File name pattern: `PREFIX_NUMBER_descriptive_name.txt`
Example: `mymod_01_grand_tournament_decision.txt`

### 3. Write an event

```pdx
namespace = mymod

mymod.0001 = {
    type = character_event
    title = mymod.0001.t
    desc = mymod.0001.desc

    trigger = {
        is_ai = no
        is_ruler = yes
    }

    immediate = {
        save_scope_as = event_ruler
    }

    option = {
        name = mymod.0001.a
        add_prestige = 100
    }
}
```

Fire from script: `trigger_event = { id = mymod.0001 days = 1 }`
Or hook via `common/on_action/` — see on-actions reference.

### 4. Write a decision

```pdx
mymod_hold_feast = {
    title = mymod_hold_feast
    desc = mymod_hold_feast_desc
    decision_group_type = major

    is_shown = { is_ruler = yes }
    is_valid = { gold >= 50 }

    effect = {
        add_gold = -50
        add_prestige = 25
        trigger_event = mymod.0001
    }

    ai_check_interval = 36
    ai_will_do = { base = 10 }
}
```

### 5. Reusable scripted logic

| Type | Folder | Invocation |
|------|--------|------------|
| Scripted trigger | `common/scripted_triggers/` | `trigger = { my_trigger = yes }` |
| Scripted effect | `common/scripted_effects/` | `my_effect = yes` |
| Script value | `common/script_values/` | `add_gold = my_value` |

Parameterized blocks use `$ARG$` substitution:

```pdx
add_flag_if_player = {
    if = {
        limit = { is_ai = no }
        add_character_flag = $FLAG$
    }
}
```

### 6. Localization

Static keys in `localization/english/<prefix>_l_english.yml`:

```yaml
l_english:
 mymod.0001.t:0 "A Grand Announcement"
 mymod.0001.desc:0 "Your court awaits your word."
 mymod.0001.a:0 "Let the celebrations begin!"
```

Encoding: UTF-8-BOM for `.txt` game files; YAML follows Paradox loc conventions.
See [reference/localization.md](reference/localization.md) for customizable loc.

### 7. Debugging checklist

```
- [ ] Unique file name with mod PREFIX (avoids silent overwrites)
- [ ] Block key is unique across loaded mods
- [ ] Localization key exists for every title/desc/option/name
- [ ] Scope is correct (character vs title vs province)
- [ ] trigger vs limit vs effect used in right context
- [ ] No references to non-existent traits, faiths, or titles
- [ ] Check game error.log after loading
```

Details: [reference/debugging.md](reference/debugging.md)

## Scripting rules (always apply)

1. **Scopes** — Every trigger/effect runs in a scope. Use `save_scope_as` for reuse.
2. **root / this / from** — `root` = initial scope; `this` = current; `from` = caller scope.
3. **trigger vs limit** — `trigger` gates the block; `limit` gates a sub-block inside effects.
4. **Load order** — Alphabetical by filename; lower NUMBER loads first, later files override.
5. **Do not invent** — Verify trigger, effect, and on_action names against vanilla data.
6. **Encoding** — Game `.txt` files use UTF-8-BOM.

## CK3-AI-IDE-Docs project integration

When working in this repository:

- **Code baseline:** `Code/CK_Ver_19/`
- **Canonical docs:** `docs/` (this skill embeds their essentials in `reference/`)
- **Staged analysis:** `docs/stages/analysis/00-workflow.md` for large doc tasks
- **JSON lookups:** `docs/triggers_detailed.json`, `effects_detailed.json`, etc.
- **Parser tools:** `Tools/run_all_parsers.py`, `Tools/analysis/run_stage.py`
- **Python env:** `bash Tools/run_analysis_env.sh`

## Anti-patterns

- Reusing vanilla filenames — causes load-order overwrites
- Deep scope nesting without `save_scope_as` — unreadable and error-prone
- Missing `exists = this` guard in dynamic trait/faith loc blocks
- Using `vassal_contracts/` path — renamed to `subject_contracts/` in recent versions
- Hardcoding character IDs instead of scopes or saved targets

## Additional resources

- [examples.md](examples.md) — trait, culture, scheme, and GUI examples
- All topic depth in `reference/` — self-contained, deployable as one directory
