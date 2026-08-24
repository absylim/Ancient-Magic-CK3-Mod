# CK3 UI and graphics

Bridge between script data, GUI layouts, and visual assets.

## Folder map

| Script-side | Asset-side |
|-------------|------------|
| `common/scripted_guis/` | `gui/` |
| `common/game_concepts/` | `gfx/interface/` |
| `common/bookmark_portraits/` | `gfx/portraits/` |
| `common/event_themes/` | `gfx/interface/illustrations/` |
| `common/event_backgrounds/` | `gfx/interface/illustrations/` |
| `common/named_colors/` | `gfx/` |
| `common/coat_of_arms/` | `gfx/coat_of_arms/` |
| `common/genes/` | `gfx/portraits/` |

## GFX structure

```
gfx/
├── interface/
│   ├── icons/
│   │   ├── traits/          # Trait icons (.dds)
│   │   ├── modifiers/       # Modifier icons
│   │   └── ...
│   └── illustrations/
│       ├── decisions/       # Decision pictures
│       ├── activities/      # Activity art
│       └── ...
├── portraits/               # Portrait modifiers, accessories
├── coat_of_arms/            # CoA textures
├── map/                     # Map textures
├── models/                  # 3D models
├── court_scene/             # Royal Court 3D assets
├── fonts/                   # Font files
└── FX/                      # Visual effects
```

### Image format

- Icons and textures: `.dds` (DirectDraw Surface)
- Recommended sizes: 64x64 for trait icons, varies for illustrations
- Reference in scripts: `gfx/interface/icons/traits/my_trait.dds`

## GUI structure

```
gui/
├── hud.gui                  # Main HUD
├── event_windows/           # Event popup layouts
├── event_window_widgets/    # Custom event widgets
├── activity_window_widgets/ # Activity UI widgets
├── window_decisions.gui     # Decisions panel
├── shared/                  # Reusable components
├── scripted_widgets/        # Data-driven widgets
└── notifications/           # Notification popups
```

### GUI language basics

GUI files use Paradox's layout language:

```gui
window = {
    name = "my_window"
    size = { 400 300 }
    position = { 100 50 }

    background = {
        texture = "gfx/interface/backgrounds/my_bg.dds"
    }

    text_single = {
        name = "title_text"
        position = { 20 20 }
        size = { 360 40 }
        text = "[GetTitle]"
    }

    button = {
        name = "close_button"
        position = { 350 10 }
        size = { 30 30 }
        onclick = "[CloseWindow]"
    }
}
```

### Data binding

GUI elements bind to game data via bracket syntax:
- `[GetTitle]` — localized title
- `[Character.GetName]` — character name
- `[GuiScope.SetRoot( Character.MakeScope ).GetScriptValue( 'value' )]"`

## Scripted GUIs

`common/scripted_guis/` — link GUI to script logic:

```pdx
mymod_custom_panel = {
    scope = character
    is_shown = { is_ruler = yes }
    effect = { }
}
```

## Event themes and backgrounds

```pdx
# common/event_themes/
my_theme = {
    background = { reference = "gfx/interface/illustrations/event_scenes/my_bg.dds" }
    icon = { reference = "gfx/interface/icons/event_icons/my_icon.dds" }
    sound = { reference = "event_court" }
}

# In event:
theme = my_theme
# Or override:
override_background = {
    trigger = { always = yes }
    reference = "gfx/interface/illustrations/event_scenes/custom.dds"
}
```

## Portraits

### Trait icons

Place at: `gfx/interface/icons/traits/<trait_key>.dds`
Reference in trait def: `icon = my_trait.dds` (relative to traits folder)

### Bookmark portraits

`common/bookmark_portraits/` — character DNA and appearance for bookmark
screens. Uses gene and portrait syntax.

### Genes and DNA

```
common/genes/       # Genetic trait visual definitions
common/dna_data/    # Predefined character DNA
```

## Coat of arms

```
common/coat_of_arms/    # CoA template definitions
gfx/coat_of_arms/       # CoA rendered textures
```

## Game concepts

`common/game_concepts/` — encyclopedia entries linked from tooltips.

```pdx
mymod_festival = {
    texture = "gfx/interface/icons/traits/festival.dds"
    shown_in_encyclopedia = yes
}
```

## Decision pictures

```pdx
picture = {
    reference = "gfx/interface/illustrations/decisions/decision_feast.dds"
}
```

## Custom event widgets

1. Create GUI file: `gui/event_window_widgets/my_widget.gui`
2. Reference in event:

```pdx
widgets = {
    widget = {
        gui = "my_widget"
        container = "background"
        controller = default
        is_shown = { always = yes }
    }
}
```

### Available controllers

| Controller | Data context | Notes |
|------------|-------------|-------|
| `default` | EventWindowWidget | No special behavior |
| `name_character` | EventWindowWidgetNameCharacter | Needs name_character_target scope |
| `text` | EventWindowWidgetEnterText | Saves text to character |
| `event_chain_progress` | EventWindowWidgetChainProgress | Needs chain length/progress scopes |
| `struggle_info` | EventWindowCustomWidgetStruggleInfo | Needs struggle start scope |

## Cross-reference workflow

```
1. Script defines data key (trait, decision, event theme)
2. Script references gfx path or icon name
3. Asset file placed at matching gfx/ path
4. GUI file (if custom window) placed in gui/
5. Localization keys for all visible text
6. Test in-game — missing assets show placeholder/magenta
```

## Checklist

```
- [ ] .dds file at path referenced in script
- [ ] Icon size appropriate for context
- [ ] GUI widget name matches file name
- [ ] Event widget controller matches GUI requirements
- [ ] Decision picture path valid
- [ ] Trait icon in gfx/interface/icons/traits/
- [ ] No hardcoded text in GUI (use loc keys)
```
