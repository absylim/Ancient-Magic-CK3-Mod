# CK3 traits

Traits define character personality, abilities, health, and appearance. Defined
in `common/traits/`.

## Trait categories

| Category | Role |
|----------|------|
| `personality` | Core personality (1–3 per character); opposites exist |
| `education` | Skill education (one at a time); levels 1–5 |
| `childhood` | Ages 3–15; converts to personality at adulthood |
| `commander` | Combat leadership traits |
| `lifestyle` | Gained through lifestyle trees; may have XP tracks |
| `health` | Diseases, wounds, mental conditions |
| `court_type` | Royal Court DLC courtier traits |
| `fame` | Renown or infamy markers |

## Basic trait definition

```pdx
mymod_festival_lord = {
    category = personality

    opposites = { shy }

    diplomacy = 2
    stewardship = 1

    same_opinion = 10
    opposite_opinion = -10

    ai_energy = low_positive_ai_value
    ai_sociability = medium_positive_ai_value

    ruler_designer_cost = 15
}
```

## Localization and icons

Defaults (overridable):

- Name key: `trait_<trait_key>`
- Description key: `trait_<trait_key>_desc`
- Icon: `gfx/interface/icons/traits/<trait_key>.dds`

### Dynamic name/description

```pdx
name = {
    first_valid = {
        triggered_desc = {
            trigger = {
                exists = this
                has_trait_xp = {
                    trait = lifestyle_physician
                    value >= 100
                }
            }
            desc = trait_physician_3
        }
        desc = trait_physician_1
    }
}
```

Always include `NOT = { exists = this }` fallback when using dynamic loc.

## Validation properties

```pdx
valid_sex = male          # all / male / female
minimum_age = 16
maximum_age = 15          # for childhood traits

potential = {
    is_female = yes       # required to receive trait
}
```

## Attribute modifiers

Any character attribute can be modified:

```pdx
diplomacy = 2
martial = -1
stewardship = 5
intrigue = 3
learning = 1
prowess = 4
health = -0.5
fertility = 0.25
```

## Opinion impacts

```pdx
same_opinion = 10
same_opinion_if_same_faith = 5
opposite_opinion = -10
attraction_opinion = 10

triggered_opinion = {
    opinion_modifier = zealous_opinion
    same_faith = yes
}
```

## Opposites and compatibility

```pdx
opposites = { craven generous }

compatibility = {
    gregarious = 20
    shy = @neg_compat_low
}
```

## Culture and faith modifiers

```pdx
culture_modifier = {
    parameter = trait_county_opinion_modifiers
    county_opinion_add = 10
}

faith_modifier = {
    parameter = great_holy_wars_active
    stewardship = 1
}
```

## Genetic traits and inheritance

```pdx
genetic = yes
good = yes                # "good" genetic trait
physical = yes

birth = 0.5               # % born with trait if not inherited
random_creation = 0.5

inherit_chance = 50         # non-genetic inheritance %
both_parent_has_trait_inherit_chance = 25
```

Genetic rules:
- Active trait inherited 100%; inactive 50%
- Both parents pass → active; one parent → inactive
- `enables_inbred = yes` allows inbred checks

## Groups and levels

```pdx
group = education_diplomacy
level = 3
flag = level_3_education
```

Groups control inheritance equivalence and mutual exclusion.

## Level tracks (XP)

```pdx
lifestyle_physician = {
    category = lifestyle
    icon = physician.dds

    learning = 1

    track = {
        50 = { learning = 1 }
        100 = { learning = 2 }
    }
}
```

Loc keys: `trait_track_<key>` / `trait_track_<key>_desc`

Effects: `add_trait_xp = { trait = lifestyle_physician value = 25 }`
Triggers: `has_trait_xp = { trait = lifestyle_physician value >= 50 }`

## Portrait and physical

```pdx
physical = yes
genetic_constraint_all = beauty_1
portrait_extremity_shift = 0.25
ugliness_portrait_extremity_shift = 0.75
```

## Special flags

| Property | Effect |
|----------|--------|
| `incapacitating = yes` | Requires regent |
| `disables_combat_leadership = yes` | Cannot lead armies |
| `can_have_children = no` | Blocks children |
| `immortal = yes` | No natural death; visual aging stops |
| `inheritance_blocker` | Blocks title inheritance |
| `add_commander_trait = yes` | Auto-generated commanders get this |
| `shown_in_encyclopedia = no` | Hide from encyclopedia |

## AI behavior

Traits modify AI personality scores:

- `ai_boldness`, `ai_rationality`, `ai_energy`, `ai_sociability`
- `ai_greed`, `ai_honor`, `ai_compassion`
- `ai_amenity_target_baseline`, `ai_amenity_spending`

## Gaining and losing traits

| Method | Mechanism |
|--------|-----------|
| Events | `add_trait` / `remove_trait` effects |
| Birth | `birth` % or genetic inheritance |
| Education | Auto-assigned at age 16 |
| Stress breaks | Personality change at high stress |
| Lifestyle | Perk trees and activities |
| Diseases | Health events and epidemics |

## File organization

```
common/traits/
├── mymod_00_traits.txt     # Your mod traits
└── _traits.info            # Vanilla property reference (if present)

gfx/interface/icons/traits/
└── mymod_festival_lord.dds
```

## Modding checklist

```
- [ ] Unique trait key (no vanilla collision)
- [ ] category set correctly
- [ ] opposites defined for conflicting traits
- [ ] Localization keys added
- [ ] Icon .dds in gfx/interface/icons/traits/
- [ ] potential trigger if trait has requirements
- [ ] Test add_trait in console: add_trait mymod_festival_lord
```
