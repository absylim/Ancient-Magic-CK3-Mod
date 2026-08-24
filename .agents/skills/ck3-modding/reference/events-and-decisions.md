# CK3 events and decisions

## Events

Primary storytelling and gameplay mechanism. Files in `events/` with namespace
prefix.

### Full event template

```pdx
namespace = mymod

mymod.0001 = {
    type = character_event
    title = mymod.0001.t
    desc = mymod.0001.desc
    theme = default

    # Optional portraits
    left_portrait = {
        character = root
        animation = personality_bold
    }
    right_portrait = scope:rival

    # Prevent re-fire
    cooldown = { years = 5 }

    trigger = {
        is_ruler = yes
        NOT = { has_character_flag = mymod_event_done }
    }

    immediate = {
        save_scope_as = event_ruler
    }

    option = {
        name = mymod.0001.a
        add_prestige = 100
        add_character_flag = mymod_event_done
    }

    option = {
        name = mymod.0001.b
        trigger = { gold >= 50 }
        add_gold = -50
        add_piety = 50
    }

    # Shown but disabled when trigger fails
    option = {
        name = mymod.0001.c
        trigger = { has_trait = brave }
        show_as_unavailable = { always = yes }
        add_martial_lifestyle_xp = 100
    }
}
```

### Firing events

```pdx
# Immediate
trigger_event = mymod.0001

# Delayed
trigger_event = { id = mymod.0001 days = 30 }

# To another character
scope:recipient = {
    trigger_event = mymod.0002
}
```

### Event scopes

| Scope | Typical use |
|-------|-------------|
| `character` | Default for character_event |
| `title` | Title-related events |
| `province` | County/province events |
| `none` | No scope setup |

Set explicitly: `scope = title` when needed.

### Portraits

```pdx
left_portrait = {
    character = scope:guest
    trigger = { is_alive = yes }
    animation = happiness
    triggered_animation = {
        trigger = { has_trait = cynical }
        animation = boredom
    }
}
```

### Custom widgets

```pdx
widgets = {
    widget = {
        gui = "my_widget"
        container = "background"
        controller = default
        is_shown = { always = yes }
        setup_scope = { }
    }
}
```

GUI file: `gui/event_window_widgets/my_widget.gui`

## Decisions

Player- and AI-facing actions in `common/decisions/`.

### Decision template

```pdx
mymod_hold_council = {
    picture = {
        reference = "gfx/interface/illustrations/decisions/decision_council.dds"
    }

    title = mymod_hold_council
    desc = mymod_hold_council_desc
    selection_tooltip = mymod_hold_council_tooltip
    decision_group_type = major

    cooldown = { years = 3 }

    is_shown = {
        is_ruler = yes
        highest_held_title_tier >= tier_duchy
    }

    is_valid = {
        custom_description = {
            text = mymod_need_gold
            gold >= 100
        }
    }

    cost = {
        gold = 100
        prestige = 50
    }

    effect = {
        add_prestige = 75
        trigger_event = mymod.0001
    }

    ai_check_interval = 36
    ai_potential = { is_ai = yes }
    ai_will_do = {
        base = 10
        modifier = {
            add = 20
            gold >= 200
        }
    }
}
```

### Key decision fields

| Field | Purpose |
|-------|---------|
| `is_shown` | Appears in decisions list |
| `is_valid` | Can be taken (shows requirements) |
| `cost` | Resources consumed on take |
| `effect` | What happens when taken |
| `cooldown` | Re-take delay |
| `decision_group_type` | UI grouping (`major`, `minor`, etc.) |
| `ai_check_interval` | Months between AI evaluations |
| `ai_will_do` | AI weight calculation |

### Decision groups

Defined in `common/decision_group_types/`. Use existing group types when
possible.

## Modifiers

Numeric bonuses/penalties applied to scopes.

### Modifier definition (`common/modifiers/`)

```pdx
mymod_festival_spirit = {
    icon = feast_positive
    diplomacy = 2
    general_opinion = 5
    monthly_prestige = 0.5
}
```

### Applying modifiers

```pdx
add_character_modifier = {
    modifier = mymod_festival_spirit
    years = 5
}

add_county_modifier = {
    modifier = mymod_prosperity
    years = 10
}

add_opinion_modifier = {
    target = scope:guest
    modifier = festival_guest_opinion
    years = 3
}
```

### Opinion modifiers (`common/opinion_modifiers/`)

```pdx
mymod_grateful = {
    opinion = 25
    years = 5
    decaying = yes
}
```

Applied via:

```pdx
add_opinion = {
    target = scope:recipient
    modifier = mymod_grateful
}
```

## Event chains and state machines

Track multi-step progression with character flags:

```pdx
# Step 1
immediate = { add_character_flag = mymod_chain_step_1 }

# Step 2 event trigger
trigger = { has_character_flag = mymod_chain_step_1 }

# Complete
effect = {
    remove_character_flag = mymod_chain_step_1
    add_character_flag = mymod_chain_complete
}
```

Use `variable` scopes for numeric progression when flags are insufficient.

## Content source and DLC

```pdx
content_source = dlc_xxx   # Shows DLC badge in event window
```

Mark debug events: `orphan = yes` to suppress unreferenced warnings.
