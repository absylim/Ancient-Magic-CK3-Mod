# CK3 modding examples

End-to-end patterns for common modding tasks.

## Example 1: Complete event chain

### Script (`events/mymod_events.txt`)

```pdx
namespace = mymod

# Intro event — fires from decision
mymod.0001 = {
    type = character_event
    title = mymod.0001.t
    desc = mymod.0001.desc
    theme = default

    left_portrait = {
        character = root
        animation = personality_rational
    }

    immediate = {
        save_scope_as = festival_host
    }

    option = {
        name = mymod.0001.a
        add_prestige = 50
        trigger_event = { id = mymod.0002 days = 30 }
    }

    option = {
        name = mymod.0001.b
        add_stress = 10
        add_character_flag = mymod_declined_festival
    }
}

# Follow-up event
mymod.0002 = {
    type = character_event
    title = mymod.0002.t
    desc = mymod.0002.desc
    theme = default

    trigger = {
        NOT = { has_character_flag = mymod_declined_festival }
    }

    option = {
        name = mymod.0002.a
        add_trait = mymod_festival_lord
        every_vassal = {
            limit = { is_powerful_vassal = yes }
            add_opinion = {
                target = root
                modifier = festival_impressed
                opinion = 15
            }
        }
    }
}
```

### Localization (`localization/english/mymod_l_english.yml`)

```yaml
l_english:
 mymod.0001.t:0 "A Festival Proposal"
 mymod.0001.desc:0 "Your steward suggests hosting a grand festival to boost morale."
 mymod.0001.a:0 "Begin preparations!"
 mymod.0001.b:0 "We cannot afford the distraction."
 mymod.0002.t:0 "Festival Success"
 mymod.0002.desc:0 "The festival was a resounding success. Your court celebrates."
 mymod.0002.a:0 "A day to remember!"
```

## Example 2: Decision with cost and AI

### Script (`common/decisions/mymod_01_festival_decision.txt`)

```pdx
mymod_hold_festival = {
    picture = {
        reference = "gfx/interface/illustrations/decisions/decision_feast.dds"
    }

    title = mymod_hold_festival
    desc = mymod_hold_festival_desc
    decision_group_type = major
    cooldown = { years = 5 }

    is_shown = {
        is_ruler = yes
        highest_held_title_tier >= tier_county
    }

    is_valid = {
        custom_description = {
            text = mymod_need_gold_for_festival
            gold >= 100
        }
    }

    cost = { gold = 100 }

    effect = {
        trigger_event = mymod.0001
    }

    ai_check_interval = 24
    ai_will_do = {
        base = 5
        modifier = {
            add = 15
            has_trait = gregarious
        }
        modifier = {
            add = -20
            has_trait = shy
        }
    }
}
```

## Example 3: Custom trait with icon

### Trait (`common/traits/mymod_00_traits.txt`)

```pdx
mymod_festival_lord = {
    category = personality

    opposites = { shy }

    diplomacy = 2
    general_opinion = 5
    monthly_prestige = 0.3

    same_opinion = 10

    ai_energy = medium_positive_ai_value
    ai_sociability = medium_positive_ai_value

    ruler_designer_cost = 20
}
```

### Modifier (`common/modifiers/mymod_00_modifiers.txt`)

```pdx
festival_impressed = {
    icon = feast_positive
    general_opinion = 10
}
```

### Icon

Place `mymod_festival_lord.dds` at `gfx/interface/icons/traits/`

## Example 4: Scripted trigger and effect

### Trigger (`common/scripted_triggers/mymod_triggers.txt`)

```pdx
mymod_can_afford_festival = {
    gold >= 100
    is_ruler = yes
    NOT = { has_character_flag = mymod_recent_festival }
}
```

### Effect (`common/scripted_effects/mymod_effects.txt`)

```pdx
mymod_festival_cleanup = {
    remove_character_flag = mymod_recent_festival
    add_character_flag = {
        flag = mymod_recent_festival
        years = 5
    }
}
```

### Usage in decision

```pdx
is_valid = { mymod_can_afford_festival = yes }
effect = {
    mymod_festival_cleanup = yes
    trigger_event = mymod.0001
}
```

## Example 5: on_action hook

### Script (`common/on_action/mymod_on_actions.txt`)

```pdx
on_birth = {
    events = {
        mymod.0100
    }
}
```

### Birth event (`events/mymod_birth_events.txt`)

```pdx
namespace = mymod

mymod.0100 = {
    type = character_event
    hidden = yes

    trigger = {
        dynasty = dynasty:mymod_special_dynasty
    }

    immediate = {
        if = {
            limit = { is_female = yes }
            add_trait = mymod_festival_lord
        }
    }
}
```

## Example 6: Script value for scaling reward

### Script (`common/script_values/mymod_values.txt`)

```pdx
mymod_festival_reward = {
    value = 50
    add = {
        value = diplomacy
        multiply = 5
    }
    min = 25
    max = 200
}
```

### Usage

```pdx
option = {
    name = mymod.0002.a
    add_prestige = mymod_festival_reward
}
```

## Example 7: Character interaction

```pdx
mymod_invite_to_festival = {
    category = interaction_category_friendly
    icon = icon_activity_feast

    is_shown = {
        scope:actor = { has_character_flag = mymod_festival_active }
        scope:recipient = {
            is_courtier_of = scope:actor
            NOT = { has_character_flag = mymod_invited_to_festival }
        }
    }

    on_accept = {
        scope:recipient = {
            add_character_flag = mymod_invited_to_festival
            add_opinion = {
                target = scope:actor
                modifier = festival_invitation
                opinion = 20
            }
        }
    }

    auto_accept = yes
}
```

## Example 8: Customizable localization

### Definition (`common/customizable_localization/mymod_loc.txt`)

```pdx
mymod_festival_greeting = {
    type = character

    text = {
        trigger = {
            has_trait = mymod_festival_lord
        }
        localization_key = mymod_greeting_festival_lord
    }
    text = {
        localization_key = mymod_greeting_default
    }
}
```

### Usage in event

```pdx
desc = {
    first_valid = {
        triggered_desc = {
            trigger = { always = yes }
            desc = mymod_festival_greeting
        }
    }
}
```

## Testing commands

```
# Fire the intro event
event mymod.0001

# Add the trait directly
add_trait mymod_festival_lord

# Check gold before decision
gold 200

# Add flag for testing
add_character_flag mymod_festival_active
```
