# CK3 mod debugging

Systematic approach to finding and fixing mod script errors.

## Error log location

CK3 writes script errors to:

- **Windows:** `Documents/Paradox Interactive/Crusader Kings III/logs/error.log`
- **Linux (Steam):** `~/.local/share/Paradox Interactive/Crusader Kings III/logs/error.log`
- **macOS:** `~/Documents/Paradox Interactive/Crusader Kings III/logs/error.log`

Clear the log, launch the game with your mod, then read new entries.

## Common error types

| Error | Typical cause |
|-------|---------------|
| `Unknown trigger/effect` | Typo or invented name; verify against vanilla |
| `Unexpected token` | Syntax error — missing brace, wrong nesting |
| `Failed to read key` | Invalid key name or wrong block structure |
| `Event/Decision not found` | Wrong namespace.id or mod not loaded |
| `Missing localization` | Key in script but not in YAML (pink text in-game) |
| `Scope mismatch` | Effect used in wrong scope (title effect on character) |
| `File conflict` | Same filename as vanilla/another mod; silent overwrite |

## Debugging workflow

```
1. Enable only your mod (+ dependencies) to isolate issues
2. Clear error.log
3. Launch to main menu (catches load-time errors)
4. Load a save or start new game (catches runtime errors)
5. Read error.log — fix top errors first (cascading errors follow)
6. Repeat until clean log
```

## In-game debug tools

### Debug mode

Launch with `-debug_mode` flag. Enables:
- Console access (backtick/tilde key)
- Right-click context menus with script options
- Additional tooltip information

### Useful console commands

```
# Traits
add_trait <trait_key>
remove_trait <trait_key>

# Resources
gold 1000
prestige 1000
piety 1000

# Events
event <namespace>.<id>

# Character
age <character_id> <years>

# Flags
add_character_flag <flag>
remove_character_flag <flag>

# Kill/revive
kill <character_id>
```

### Script logging

Add temporary logging to trace execution:

```pdx
debug_log = "mymod: event fired for character"
debug_log_scopes = yes
```

Remove debug logging before release.

## Validation checklist

### File-level

```
- [ ] UTF-8-BOM encoding on .txt files
- [ ] Unique filename with mod PREFIX
- [ ] File in correct folder
- [ ] Opening/closing braces balanced
- [ ] No tabs mixed with spaces (use spaces)
```

### Script-level

```
- [ ] Block key is globally unique
- [ ] All trigger/effect names verified in vanilla
- [ ] Scopes correct for each effect
- [ ] save_scope_as before scope:name reference
- [ ] trigger vs limit used correctly
- [ ] Parameterized $ARG$ blocks called with all args
```

### Content-level

```
- [ ] Every visible string has localization key
- [ ] Trait/faith/culture/title references exist
- [ ] GFX paths point to existing .dds files
- [ ] Event namespace matches file namespace
- [ ] Decision is_valid shows helpful custom_description
```

## Load order debugging

When your content does not appear:

1. Check mod is enabled in launcher
2. Check mod load order (later mods override earlier)
3. Check filename sort order within your mod (NUMBER in filename)
4. Check for filename collision with vanilla or other mods
5. Verify block key is not overridden by another loaded file

## Scope debugging

When effects hit wrong targets:

```pdx
# Add temporary scope logging
debug_log = "root is: [ROOT.GetCharacter.GetName]"
scope:target = {
    debug_log = "target is: [THIS.GetCharacter.GetName]"
}
```

Common scope mistakes:
- Using `root` when `scope:recipient` is needed
- Forgetting to `save_scope_as` before referencing
- Running title effects while in character scope

## Event debugging

Events not firing:

1. Check `trigger` conditions — test each in isolation
2. Check `cooldown` — may be blocking re-fire
3. Check on_action hook name is correct
4. Check `trigger_event` ID matches namespace.id
5. Use `orphan = yes` temporarily to suppress unreferenced warnings
6. Fire manually via console: `event mymod.0001`

## Performance considerations

- Avoid `every_realm_province` in frequently-fired on_actions
- Use `random_list` with weights instead of many `if` chains
- Cache values in script_values instead of recalculating
- Limit event frequency with cooldowns

## Isolating mod conflicts

Binary search with mods:
1. Disable half your mods
2. If error persists, problem is in enabled half
3. Repeat until single mod identified
4. Within mod, disable half the files (rename extension)

## Release checklist

```
- [ ] Clean error.log with only this mod enabled
- [ ] All debug_log statements removed
- [ ] No orphan = yes on production events (unless intentional)
- [ ] Localization complete for all languages you support
- [ ] Mod descriptor (.mod file) has correct version and paths
- [ ] Tested on new game start and mid-game save
```
