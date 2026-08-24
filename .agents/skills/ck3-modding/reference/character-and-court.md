# CK3 character and court

Character interactions, court positions, hooks, secrets, and guest management.

## Key folders

| Folder | Content |
|--------|---------|
| `character_interactions/` | Player/AI actions between characters |
| `character_interaction_categories/` | UI grouping for interactions |
| `court_positions/` | Appointable court roles |
| `court_types/` | Royal Court types and bonuses |
| `court_amenities/` | Court amenity levels |
| `hook_types/` | Hook categories (weak, strong, etc.) |
| `secret_types/` | Secret definitions |
| `important_actions/` | Notification action prompts |
| `guest_system/` | Guest arrival/departure |
| `courtier_guest_management/` | Courtier management rules |

## Character interactions

Interactions define actor → recipient action flows.

```pdx
mymod_demand_tribute = {
    category = interaction_category_vassal
    icon = icon_demand_payment

    is_shown = {
        scope:actor = { is_independent_ruler = yes }
        scope:recipient = {
            is_vassal_of = scope:actor
        }
    }

    is_valid = {
        scope:recipient = { is_imprisoned = no }
        custom_description = {
            text = mymod_not_recently_tributized
            NOT = { scope:recipient = { has_character_flag = recent_tribute } }
        }
    }

    on_accept = {
        scope:recipient = {
            pay_short_term_gold = {
                target = scope:actor
                gold = 50
            }
            add_character_flag = {
                flag = recent_tribute
                years = 3
            }
        }
        scope:actor = {
            add_prestige = 25
        }
    }

    auto_accept = {
        custom_description = {
            text = mymod_intimidated
            scope:recipient = {
                dread_modified_intimidated = scope:actor
            }
        }
    }

    ai_accept = {
        base = 0
        modifier = {
            add = 20
            scope:recipient = { has_trait = craven }
        }
        modifier = {
            add = -30
            scope:recipient = { has_trait = brave }
        }
    }
}
```

### Interaction key fields

| Field | Purpose |
|-------|---------|
| `is_shown` | Interaction appears in menu |
| `is_valid` | Interaction can be used |
| `on_accept` | Effects when accepted |
| `on_decline` | Effects when declined |
| `auto_accept` | Conditions for automatic acceptance |
| `ai_accept` / `ai_will_do` | AI acceptance weights |
| `cost` | Resource cost to actor |

### Scopes in interactions

- `scope:actor` — character initiating
- `scope:recipient` — target character

## Court positions

Appointable roles granting ongoing modifiers.

```pdx
mymod_court_jester = {
    sort_order = 400
    max_available = 1

    aptitude = {
        value = 1
        add = {
            value = diplomacy
            multiply = 0.5
        }
    }

    is_shown = {
        has_dlc_feature = royal_court
    }

    is_shown_character = {
        is_courtier_of = scope:liege
    }

    is_valid_show = {
        custom_description = {
            text = mymod_needs_court
            scope:liege = { has_royal_court = yes }
        }
    }

    on_court_position_received = {
        add_character_modifier = {
            modifier = mymod_jester_modifier
        }
    }

    on_court_position_revoked = {
        remove_character_modifier = mymod_jester_modifier
    }
}
```

## Hooks

Hooks represent leverage over characters.

```pdx
# Grant a hook via effect:
add_hook = {
    type = favor_hook
    target = scope:liege
}

# Hook types defined in common/hook_types/
```

Common hook types: `favor_hook`, `strong_hook`, `weak_hook`, `life_threat_hook`

## Secrets

```pdx
# Reveal or create secrets via effects
give_secret = { type = secret_deviant target = scope:rival }
expose_secret = scope:secret_target
```

Secret types in `common/secret_types/`.

## Guest system

Controls which characters arrive as guests at courts. Modified via
`guest_system/` and `courtier_guest_management/`.

## Character memories

`character_memory_types/` — defines memory categories characters can hold.
Used by event system for narrative callbacks.

## Pool character selectors

`pool_character_selectors/` — rules for generating characters from pools
(e.g., marriage candidates, mercenary leaders).

## Court types (Royal Court DLC)

```pdx
# common/court_types/
# Defines court grandeur levels and bonuses
# Courtiers gain court_type traits at high grandeur
```

## Scripting patterns

### Appoint court position

```pdx
appoint_court_position = {
    recipient = scope:candidate
    court_position = mymod_court_jester
}
```

### Imprison and release

```pdx
imprison = {
    target = scope:prisoner
    type = house_arrest
}

release_from_prison = scope:prisoner
```

### Character templates

`scripted_character_templates/` — reusable character generation:

```pdx
create_character = {
    template = mymod_merchant_template
    location = scope:capital
    save_scope_as = new_merchant
}
```

## Checklist

```
- [ ] Interaction scopes (actor/recipient) used correctly
- [ ] is_shown and is_valid separated (show vs enable)
- [ ] ai_accept weights tested for reasonable AI behavior
- [ ] Court position on_received/on_revoked modifiers balanced
- [ ] Hook types exist in common/hook_types/
- [ ] Localization for interaction name and description
```
