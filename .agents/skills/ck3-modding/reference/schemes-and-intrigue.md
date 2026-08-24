# CK3 schemes and intrigue

Schemes, factions, and lifestyle perks define intrigue gameplay.

## Schemes

Defined in `common/schemes/`. Schemes are long-running plots against
characters.

### Scheme structure

```pdx
mymod_slander = {
    category = personal
    icon = icon_scheme_slander

    allow = {
        is_adult = yes
        NOT = { is_at_war_with = scope:target }
    }

    valid = {
        scope:target = { is_alive = yes }
        custom_description = {
            text = mymod_slander_cooldown
            NOT = { has_character_flag = recent_slander }
        }
    }

    base_success_chance = {
        value = 25
        add = {
            value = intrigue
            multiply = 2
        }
    }

    base_secrecy = {
        value = 50
    }

    on_phase_completed = {
        # Progress effects
    }

    on_monthly = {
        # Ongoing effects during scheme
    }

    on_success = {
        scope:target = {
            add_character_modifier = {
                modifier = mymod_slandered
                years = 3
            }
        }
    }

    on_discovered = {
        scope:owner = {
            add_tyranny = minor_tyranny_gain
        }
    }

    on_failure = {
        scope:owner = {
            add_stress = 20
        }
    }
}
```

### Scheme key concepts

| Concept | Description |
|---------|-------------|
| Owner | Character running the scheme |
| Target | Character being schemed against |
| Agents | Characters assisting the scheme |
| Success chance | Probability of completion |
| Secrecy | Chance of remaining hidden |
| Phases | Progress stages before resolution |

### Scheme folders

```
common/schemes/
├── scheme_types/       # Individual scheme definitions
└── ...
```

## Factions

Defined in `common/factions/`. Vassal coalitions against their liege.

```pdx
mymod_independence_faction = {
    sort_order = 100

    is_valid = {
        scope:faction_target = { is_independent_ruler = yes }
        NOT = { is_vassal_of = scope:faction_target }
    }

    can_character_join = {
        is_vassal_of = scope:faction_target
        NOT = { has_truce = scope:faction_target }
    }

    power_threshold = 0.8

    on_creation = { }
    on_disband = { }
    on_faction_war = {
        scope:faction_leader = {
            start_war = {
                casus_belli = mymod_independence_cb
                target = scope:faction_target
            }
        }
    }
}
```

### Faction key fields

| Field | Purpose |
|-------|---------|
| `can_character_join` | Who can join |
| `power_threshold` | Military strength needed to press demands |
| `on_faction_war` | War declaration when faction presses |
| `discontent` | Faction anger level |

## Lifestyles and perks

```
common/lifestyles/      # Lifestyle tree definitions
common/focuses/         # Focus options within lifestyles
common/lifestyle_perks/ # Individual perk definitions
```

### Lifestyle perk pattern

```pdx
mymod_intrigue_perk = {
    lifestyle = intrigue_lifestyle
    tree = intrigue_temptation
    position = { 1 2 }

    effect = {
        add_intrigue_lifestyle_xp = 100
    }

    trait = deceitful           # Optional trait requirement
    can_be_picked = {
        has_lifestyle = intrigue_lifestyle
    }
}
```

## Stress system integration

Intrigue actions often interact with stress:

```pdx
add_stress = 15
stress_impact = {
    base = medium_stress_impact_gain
    sadistic = minor_stress_impact_loss
    compassionate = major_stress_impact_gain
}
```

## Secrets in intrigue

Schemes can create and expose secrets:

```pdx
# Blackmail scheme uses secrets as leverage
# give_secret and expose_secret effects
```

## Related event folders

```
events/scheme_events/     # Scheme progress and outcome events
events/secret_events/     # Secret discovery events
events/factions/          # Faction-related events
events/blackmail_events.txt
```

## Activities (intrigue-adjacent)

`common/activities/` — hosted activities (feasts, hunts, tours) with their
own event chains. See activity definitions for intrigue opportunities.

## Scripting scheme start

```pdx
start_scheme = {
    type = mymod_slander
    target = scope:rival
}
```

## Checklist

```
- [ ] Scheme allow/valid triggers tested for edge cases
- [ ] Success/secrecy calculations balanced
- [ ] on_discovered consequences defined
- [ ] Faction power_threshold reasonable
- [ ] CB referenced in faction war exists
- [ ] Lifestyle perks have valid tree position
- [ ] Events for scheme outcomes written
```
