# CK3 scripting primitives

CK3 uses Paradox (PDX) script: nested key-value blocks in `.txt` files.

## Block syntax

```pdx
block_name = {
    key = value
    nested = {
        child = 10
    }
}
```

### Value types

- **Scalars:** `yes`, `no`, integers, floats, quoted strings
- **Lists:** `parents = { culture_a culture_b }`
- **References:** `faith:catholic`, `culture:english`, `title:k_england`
- **Scoped blocks:** nest inside the active scope

## trigger vs limit vs effect

| Keyword | Role |
|---------|------|
| `trigger` | Gate whether a block may run (events, decisions, `is_shown`) |
| `limit` | Gate a sub-block inside an effect or iterator |
| `effect` / `immediate` / `option` | Actions that mutate game state |

```pdx
# trigger gates the event
trigger = { gold >= 100 }

# limit gates a branch inside an effect
if = {
    limit = { has_trait = brave }
    add_prestige = 50
}
```

## Scopes

Scopes define execution context for triggers and effects.

### Core scope types

`character`, `landed_title`, `province`, `faith`, `culture`, `dynasty`, `war`,
`scheme`, `activity`, `secret`, `combat`, `combat_side`, `casus_belli`, `story`

### Scope operators

| Operator | Meaning |
|----------|---------|
| `root` | Initial scope when script started |
| `this` | Current scope |
| `from` | Previous scope (one level back) |
| `fromfrom` | Two scopes back |
| `scope:name` | Saved scope reference |
| `save_scope_as = name` | Persist scope for later reference |
| `save_temporary_scope_as = name` | Scope lasts for current script only |

### Scope switching

```pdx
# In character scope:
realm = {
    capital_county = {
        holder = {
            add_gold = 100
        }
    }
}
```

### Event targets

Common implicit scopes in events:

- `root` — primary event recipient
- `from` — character who triggered the event
- `fromfrom` — trigger of the trigger
- `actor` / `recipient` — interaction scopes

## Triggers

Boolean conditions returning yes/no.

```pdx
trigger = {
    is_ai = no
    gold >= 100
    age >= 16
    has_trait = brave
    faith = faith:catholic
    any_vassal = {
        opinion = { target = root value >= 20 }
    }
}
```

### Logic operators

```pdx
AND = { is_adult = yes is_ruler = yes }
OR = { has_trait = brave has_trait = zealous }
NOT = { has_trait = craven }
NOR = { has_trait = craven has_trait = lazy }
```

### Iteration triggers

```pdx
any_vassal = { count >= 2 is_powerful_vassal = yes }
every_child = { is_adult = yes }
random_courtier = {
    limit = { is_female = yes }
    # effects if used in effect context
}
```

### Frequently used triggers

| Trigger | Checks |
|---------|--------|
| `is_ai` | Character is AI-controlled |
| `is_alive` | Character is alive |
| `is_ruler` | Holds any title |
| `has_trait` | Has trait or trait group |
| `has_government` | Specific government type |
| `exists` | Scope target is not null |
| `has_character_flag` | Flag is set |
| `gold` / `prestige` / `piety` / `stress` | Resource comparisons |
| `age` | Character age comparison |

## Effects

Actions that change game state.

```pdx
effect = {
    add_gold = 100
    add_prestige = 50
    add_trait = brave
    remove_trait = craven
    add_character_flag = my_flag
    scope:recipient = {
        add_opinion = {
            target = root
            modifier = grateful_opinion
            opinion = 25
        }
    }
}
```

### Common character effects

`add_gold`, `add_prestige`, `add_piety`, `add_stress`, `add_trait`,
`remove_trait`, `add_character_flag`, `remove_character_flag`, `change_culture`,
`change_faith`, `set_relation_friend`, `imprison`, `release_from_prison`

### Title effects

`gain_title`, `destroy_title`, `create_title`, `change_development_level`

### Event effects

```pdx
trigger_event = { id = mymod.0002 days = 10 }
hidden_effect = { add_gold = 100 }
send_interface_message = {
    type = event_generic
    title = mymod_msg.t
    desc = mymod_msg.desc
}
```

### Scoped iteration effects

`every_vassal`, `every_realm_county`, `every_realm_province`, `random_courtier`,
`ordered_vassal` (sort before applying)

```pdx
every_vassal = {
    limit = { is_powerful_vassal = yes }
    add_opinion = {
        target = root
        modifier = impressed_opinion
        opinion = 10
    }
}
```

### Conditional effects

```pdx
if = {
    limit = { prowess > 10 }
    add_trait = brave
}
else_if = {
    limit = { diplomacy > 10 }
    add_trait = gregarious
}
else = {
    add_trait = humble
}
```

## Scripted reuse

### Scripted triggers (`common/scripted_triggers/`)

```pdx
# Definition:
can_hold_tournament = {
    is_ruler = yes
    gold > 100
    NOT = { has_character_flag = recent_tournament }
}

# Usage:
trigger = { can_hold_tournament = yes }
```

### Scripted effects (`common/scripted_effects/`)

```pdx
# Definition:
give_reward = {
    add_prestige = 50
    add_gold = 25
}

# Usage:
give_reward = yes
```

### Parameterized blocks

Use `$ARG$` for substitution:

```pdx
add_flag_if_player = {
    if = {
        limit = { is_ai = no }
        add_character_flag = $FLAG$
    }
}

# Call:
add_flag_if_player = { FLAG = my_custom_flag }
```

## Modifiers (in-script application)

```pdx
add_character_modifier = {
    modifier = my_modifier
    years = 5
}

add_county_modifier = {
    modifier = prosperity_modifier
    years = 10
}
```

Modifier definitions live in `common/modifiers/`. See `realm-systems.md` and
`events-and-decisions.md` for modifier block patterns.

## Best practices

1. Save complex scopes with `save_scope_as` instead of deep nesting
2. Comment non-obvious logic with `#` comments
3. Guard dynamic loc with `exists = this` fallbacks
4. Extract repeated logic into scripted triggers/effects
5. Never invent trigger or effect names — verify against vanilla
6. Use `hidden_effect` for behind-the-scenes changes without player messages
