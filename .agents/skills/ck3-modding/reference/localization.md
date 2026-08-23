# CK3 localization

All player-visible text uses localization keys. Static text lives in YAML;
dynamic text uses helper systems in `common/`.

## Static localization

### File location

`localization/english/<prefix>_l_english.yml` (and other language folders).

### Format

```yaml
l_english:
 mymod.0001.t:0 "A Grand Announcement"
 mymod.0001.desc:0 "Your court awaits your word."
 mymod.0001.a:0 "Let the celebrations begin!"
 mymod_hold_council:0 "Hold a Grand Council"
 mymod_hold_council_desc:0 "Gather your vassals for counsel."
 trait_mymod_trait:0 "Festival Lord"
 trait_mymod_trait_desc:0 "Known for hosting magnificent celebrations."
```

### Key rules

- Header: `l_english:` (or `l_french:`, etc.)
- Key format: `key:0 "Text"` — version suffix `:0` is standard
- Indentation: one space before keys (Paradox YAML convention)
- Reference keys in scripts without the `:0` suffix: `title = mymod.0001.t`
- Use UTF-8 encoding for YAML files

### Naming conventions

| Content | Key pattern |
|---------|-------------|
| Event title | `<namespace>.<id>.t` |
| Event description | `<namespace>.<id>.desc` |
| Event option | `<namespace>.<id>.a` / `.b` / `.c` |
| Decision title | `<decision_key>` |
| Decision description | `<decision_key>_desc` |
| Trait name | `trait_<trait_key>` |
| Trait description | `trait_<trait_key>_desc` |
| Modifier | `<modifier_key>` |
| Trait track | `trait_track_<key>` / `trait_track_<key>_desc` |

## Customizable localization

`common/customizable_localization/` — pick text variants using triggers.

```pdx
mymod_greeting = {
    type = character

    text = {
        trigger = { is_female = yes }
        localization_key = mymod_greeting_female
    }
    text = {
        localization_key = mymod_greeting_male
    }
}
```

Usage in events: `desc = { first_valid = { mymod_greeting } }`

### Common patterns

```pdx
# first_valid — first passing trigger wins
name = {
    first_valid = {
        triggered_desc = {
            trigger = { gold >= 1000 }
            desc = mymod_rich_greeting
        }
        desc = mymod_poor_greeting
    }
}
```

## Trigger localization

`common/trigger_localization/` — formats tooltip text for scripted conditions.

Used by decisions and interactions to show why something is valid/invalid.

## Effect localization

`common/effect_localization/` — formats tooltip text for scripted outcomes.

Shows players what an effect will do before they commit.

## Flavorization

`common/flavorization/` — dynamic title and name formatting.

Controls how titles, names, and references appear based on culture, faith,
gender, etc.

## Messages

`common/messages/` — notification message types shown via
`send_interface_message`.

## Localization helpers checklist

```
- [ ] Every title, desc, option, and tooltip has a loc key
- [ ] Trait keys follow trait_<key> / trait_<key>_desc convention
- [ ] Decision keys match decision block name
- [ ] No hardcoded English in .txt script files
- [ ] Dynamic loc has fallback for NOT = { exists = this }
- [ ] Test with missing loc — game shows key name in pink/magenta
```

## Multi-language support

Copy English YAML to other language folders and translate values. Keep keys
identical across all language files.

```
localization/
├── english/mymod_l_english.yml
├── french/mymod_l_french.yml
└── german/mymod_l_german.yml
```

## Common mistakes

- Forgetting `:0` version suffix in YAML (causes load errors)
- Mismatched key between script and YAML
- Using tabs instead of spaces in YAML
- Missing `_desc` suffix for descriptions
- Dynamic trait/faith names without `exists = this` guard
