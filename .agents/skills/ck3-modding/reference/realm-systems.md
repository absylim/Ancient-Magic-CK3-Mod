# CK3 realm systems

Government, laws, warfare, and succession mechanics.

## Government

Defined in `common/governments/`. Government type controls available laws,
succession, contracts, and character interactions.

### Government definition pattern

```pdx
my_government = {
    government_rules = {
        create_cadet_branches = yes
        religious = yes
        court_generate_spouses = yes
    }

    primary_holding = castle_holding
    valid_holdings = { castle_holding city_holding }
    required_county_holdings = { castle_holding }

    can_get_government = {
        NOT = { government_has_flag = government_is_landless }
    }

    character_modifier = {
        vassal_limit = 20
    }
}
```

### Key government folders

| Folder | Content |
|--------|---------|
| `governments/` | Government type definitions |
| `laws/` | Law groups and individual laws |
| `council_positions/` | Council role definitions |
| `council_tasks/` | Council task assignments |
| `subject_contracts/` | Vassal/subject obligations (v19+) |
| `diarchies/` | Co-ruler systems |
| `legitimacy/` | Legitimacy levels and effects |
| `court_types/` | Royal Court types |
| `domiciles/` | Landless adventurer camps |

### Version note

`vassal_contracts/` was renamed to `subject_contracts/` in recent versions.

## Laws

Defined in `common/laws/`. Laws are grouped; rulers enact one law per group.

```pdx
crown_authority_2 = {
    group = crown_authority
    level = 2

    is_valid = {
        OR = {
            has_realm_law = crown_authority_1
            has_realm_law = crown_authority_0
        }
    }

    can_pass = {
        custom_description = {
            text = "needs_crown_authority_1"
            has_realm_law = crown_authority_1
        }
    }

    modifier = {
        vassal_opinion = -10
        domain_limit = 1
    }

    flag = increased_crown_authority
}
```

### Common law groups

- Succession laws (`succession_gender_laws`, etc.)
- Crown authority / tribal authority / tribal cohesion
- Title revocation laws
- Tax laws (for administrative governments)
- Realm priest laws

### Passing laws

Usually via decisions or character interactions. Effects use `add_realm_law` or
`set_realm_law`.

## Warfare

### Casus belli (`common/casus_belli_types/`)

```pdx
mymod_conquest_cb = {
    icon = conquest
    group = conquest_group

    allowed_for_character = {
        is_independent_ruler = yes
    }

    allowed_against_character = {
        NOT = { is_allied_to = scope:attacker }
    }

    cost = {
        piety = 100
    }

    on_declaration = {
        on_declared_war = yes
    }

    on_victory_desc = { ... }
    on_white_peace_desc = { ... }
    on_defeat_desc = { ... }
}
```

### Men-at-Arms (`common/men_at_arms_types/`)

```pdx
mymod_huscarls = {
    type = skirmishers
    can_recruit = {
        culture = { has_cultural_tradition = tradition_warrior_culture }
    }
    damage = 22
    toughness = 16
    pursuit = 10
    screen = 14
    siege_value = 0.1
    max_regiments = 5
    buy_cost = { gold = 115 }
    low_maintenance_cost = { gold = 0.30 }
    high_maintenance_cost = { gold = 0.90 }
}
```

### War-related folders

| Folder | Content |
|--------|---------|
| `casus_belli_types/` | War justification types |
| `casus_belli_groups/` | CB categorization |
| `men_at_arms_types/` | Regiment types |
| `combat_effects/` | Battle modifiers |
| `combat_phase_events/` | Combat phase events |
| `raids/` | Raiding configuration |
| `ai_war_stances/` | AI war behavior |

### Starting wars (script)

```pdx
start_war = {
    casus_belli = mymod_conquest_cb
    target = scope:enemy
    target_title = title:d_target
}
```

## Succession

### Election (`common/succession_election/`)

Defines electoral systems: who votes, weights, candidates.

### Appointment (`common/succession_appointment/`)

Defines appointment-based succession (administrative governments).

### Succession laws

Set via realm laws. Common types:
- Single heir (primogeniture, ultimogeniture)
- Multiple heirs (partition variants)
- Elective (various elective types)
- Appointment (administrative)
- Clan (house-based)
- Tribal (opinion-based)

### Script effects

```pdx
set_designated_heir = scope:heir
remove_designated_heir = yes
change_succession_law = single_heir_succession_law
```

## Modifiers in realm context

```pdx
# common/modifiers/
mymod_crown_authority_bonus = {
    icon = crown_positive
    vassal_opinion = -5
    domain_limit = 1
    monthly_prestige = 0.2
}
```

## Council

```pdx
# Council positions define roles
# Council tasks define what councillors do
# Appointed via character interactions or script:
appoint_court_position = {
    recipient = scope:councillor
    court_position = councillor_chancellor
}
```

## Checklist

```
- [ ] Government type has valid holdings defined
- [ ] Laws reference correct group names
- [ ] CB targets valid title tiers
- [ ] MaA type has valid can_recruit triggers
- [ ] Succession files match government type
- [ ] subject_contracts/ used (not vassal_contracts/)
```
