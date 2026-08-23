# CK3 culture and faith

## Culture

Cultures define customs, technologies, military units, and social practices.
Defined in `common/culture/cultures/`.

### Basic culture definition

```pdx
mymod_culture = {
    color = { 0.8 0.2 0.1 }

    # For hybrid/divergent cultures:
    parents = { english norman }
    created = 1066.1.1

    ethos = ethos_bellicose
    heritage = heritage_frankish
    language = language_frankish
    martial_custom = martial_custom_male_only
    head_determination = head_determination_domain

    traditions = {
        tradition_warrior_culture
        tradition_hereditary_hierarchy
    }

    name_list = name_list_english
}
```

### Cultural pillars

| Pillar | Effect |
|--------|--------|
| `ethos` | Core values; affects modifiers and court types |
| `heritage` | Origin group; affects opinions and hybridization |
| `language` | Naming, title names, language learning |
| `martial_custom` | Who can be commanders/knights (Royal Court DLC) |
| `head_determination` | How culture head is chosen |

### Traditions

```pdx
traditions = {
    tradition_warrior_culture
    tradition_fp1_coastal_warriors
}

# DLC-gated tradition
dlc_tradition = {
    trait = tradition_ep3_audacious_cadets
    requires_dlc_flag = roads_to_power
}
```

Traditions grant modifiers, unlock mechanics, and affect AI behavior.

### Innovations

Innovations are defined separately in `common/culture/innovations/`. Cultures
discover innovations over time; culture head selects Fascination.

### Culture creation types

| Type | Mechanism |
|------|-----------|
| Diverge | Single parent culture splits |
| Hybrid | Two parent cultures merge |
| Historical | `created` date for bookmark accuracy |

### Culture effects on gameplay

- Opinion modifiers between cultures (worse across heritages)
- Men-at-Arms types unlocked by traditions/innovations
- Building restrictions and bonuses
- Court type availability (from ethos)
- Succession law options (via traditions)
- Visual: architecture, clothing, CoA style

### Key folders

```
common/culture/
├── cultures/           # Culture definitions
├── traditions/         # Tradition definitions
├── innovations/        # Innovation definitions
├── pillars/            # Ethos, heritage, language, etc.
├── name_lists/         # Character naming
├── eras/               # Innovation eras
└── aesthetics/         # Visual styles
```

## Faith

Faiths are denominations within religions. Defined in `common/religion/faiths/`.

### Religion hierarchy

1. **Religion family** — broadest (`rf_abrahamic`, `rf_pagan`, `rf_eastern`)
2. **Religion** — tradition (`christianity`, `islam`, `norse_pagan`)
3. **Faith** — denomination (`catholic`, `orthodox`, `sunni`)

### Religion family structure

```pdx
# common/religion/religion_families/
rf_abrahamic = {
    is_pagan = no
    piety_icon_group = "christian"
    graphical_faith = "catholic_gfx"
    hostility_doctrine = abrahamic_hostility_doctrine
}
```

### Religion structure

```pdx
# common/religion/religions/
christianity = {
    family = rf_abrahamic
    graphical_faith = "catholic_gfx"
    piety_icon_group = "christian"

    doctrine = doctrine_spiritual_head
    doctrine = doctrine_monogamy

    traits = {
        virtues = { temperate chaste }
        sins = { gluttonous lustful }
    }

    holy_order_names = { ... }
    holy_order_maa = { ... }
}
```

### Faith structure

```pdx
# common/religion/faiths/
mymod_faith = {
    color = { 0.9 0.8 0.1 }
    icon = "mymod_faith_icon"

    religion = christianity

    # Doctrines
    doctrine = doctrine_theocracy_lay_clergy
    doctrine = doctrine_gender_equal
    doctrine = doctrine_divorce_allowed
    doctrine = doctrine_bastardry_none
    doctrine = doctrine_consanguinity_restricted
    doctrine = doctrine_homosexuality_shunned

    # Core tenets (pick 3)
    doctrine = tenet_communal_identity
    doctrine = tenet_legalism
    doctrine = tenet_ritual_celebrations

    # Holy sites (barony title keys)
    holy_site = jerusalem
    holy_site = rome
    holy_site = constantinople

    # Character modifiers
    piety_level_1 = { learning = 1 }
}
```

### Key faith concepts

| Concept | Description |
|---------|-------------|
| Doctrines | Rules defining faith behavior |
| Core tenets | 3 special doctrines with unique bonuses |
| Holy sites | Sacred baronies; required for some actions |
| Fervor | 0–100% measure of faith righteousness |
| Hostility | Righteous / Astray / Hostile / Evil toward other faiths |
| Organization | Organized (reformed) vs unreformed |

### Doctrine precedence

Faith → Religion → Family. Lower levels override higher defaults.

### Faith hostility

Hostility between faiths determines:
- Holy war validity
- Marriage acceptance
- Opinion modifiers
- Conversion speed

### Religion key folders

```
common/religion/
├── religion_families/
├── religions/
├── faiths/
├── doctrines/          # All doctrine definitions
├── holy_sites/         # Holy site barony mappings
└── great_holy_wars/    # GHW configuration
```

## Culture-faith interaction

- Culture and faith together determine many opinion modifiers
- Some government types require specific culture/faith combinations
- Conversion: `change_faith = faith:mymod_faith`
- Culture change: `change_culture = culture:mymod_culture`
- Some traditions reference faith doctrines via `doctrine_parameter`

## Modding checklist

### Culture
```
- [ ] Unique culture key
- [ ] All five pillars assigned
- [ ] Traditions exist in common/culture/traditions/
- [ ] name_list referenced and defined
- [ ] Localization for culture name and tradition names
- [ ] color defined for map display
```

### Faith
```
- [ ] Unique faith key
- [ ] religion parent set
- [ ] 3 core tenets selected
- [ ] Holy sites are valid barony title keys
- [ ] Doctrines exist in common/religion/doctrines/
- [ ] Icon defined (gfx + faith icon reference)
- [ ] Localization for faith name and doctrine descriptions
```
