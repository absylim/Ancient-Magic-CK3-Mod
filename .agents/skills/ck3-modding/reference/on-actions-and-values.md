# CK3 on_actions and script values

## on_actions

on_actions hook engine lifecycle events. Files live in `common/on_action/`.

### Syntax

```pdx
yearly_global_pulse = {
    effect = {
        # runs when this on_action fires
    }
}
```

### Chaining events

```pdx
on_birth = {
    events = {
        birth_events.0001
    }
    random_events = {
        chance_to_fire = {
            value = 0.5
        }
        events = {
            birth_events.0002
            birth_events.0003
        }
    }
    on_action = {
        on_action = dynasty_birth_on_actions
    }
}
```

### Relationship to events

- on_actions run on engine-timed or state-change hooks
- `trigger_event` inside an on_action starts an event from `events/`
- Event files use `namespace` and IDs like `namespace.0001`
- Events failing from on_actions do **not** run `on_trigger_fail`

### Common hook categories

| Category | Examples |
|----------|----------|
| Character lifecycle | `on_birth`, `on_death`, `on_marriage`, `on_divorce` |
| Yearly pulses | `yearly_global_pulse`, `yearly_playable_pulse` |
| Rank changes | `on_rank_up`, `on_rank_down` |
| War | `on_war_started`, `on_war_ended` |
| Stress | `stress_loss_*`, `stress_gain_*` |
| Traditions | `on_tradition_added`, `on_tradition_removed` |
| Schemes | `on_scheme_*` variants |
| Activities | `feast_*`, `hunt_*`, `pilgrimage_*` |

Search vanilla `common/on_action/` for the exact hook name before using.

### Example

```pdx
# common/on_action/mymod_on_actions.txt
yearly_playable_pulse = {
    events = {
        mymod.0100
    }
}
```

## Script values

Named formulas used wherever a numeric field is expected. Files live in
`common/script_values/`.

### Syntax

```pdx
my_reward_value = {
    value = 10
    if = {
        limit = { has_trait = greedy }
        add = 5
    }
    if = {
        limit = { is_ruler = yes }
        multiply = 1.5
    }
    min = 0
    max = 100
}
```

### Usage

```pdx
add_gold = my_reward_value

cooldown = { years = my_reward_value }

random_list = {
    50 = { add_prestige = my_reward_value }
    50 = { }
}
```

### Operations

| Operation | Effect |
|-----------|--------|
| `value = N` | Base value |
| `add = N` or `add = other_value` | Add to result |
| `subtract = N` | Subtract |
| `multiply = N` | Multiply |
| `divide = N` | Divide |
| `min = N` / `max = N` | Clamp result |
| `if = { limit = {} add = N }` | Conditional branch |
| `round = yes` | Round to integer |

### Referencing other values

Script values can reference character properties:

```pdx
prowess_bonus = {
    value = prowess
    multiply = 2
}
```

## Scripted modifiers

`common/scripted_modifiers/` — dynamic modifier calculations that scale with
game state. Used where static modifier values are insufficient.

## Scripted lists

`common/scripted_lists/` — named lists of scopes for reuse in triggers and
effects.

## Tips

- Prefer on_actions over polling with `trigger_event` loops
- Use script values for anything repeated or balance-tuned
- Keep on_action additions in separate prefixed files to avoid merge conflicts
- Test on_action hooks with short `cooldown` on test events first
