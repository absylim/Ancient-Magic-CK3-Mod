---
status: draft
audience: dev-design
code_roots:
  - common/script_values/01_ancient_magic_mana_system_values.txt
  - common/on_action/01_ancient_magic_mana_system_on_actions.txt
  - common/scripted_effects/01_ancient_magic_spell_system_effects.txt
  - common/scripted_effects/01_ancient_magic_effects.txt
  - common/script_values/01_ancient_magic_spell_cost_values.txt
  - common/scripted_guis/01_ancient_magic_hud_sguis.txt
  - common/scripted_triggers/01_ancient_magic_spell_triggers.txt
  - common/game_rules/01_ancient_magic_game_rules.txt
  - gui/custom_gui/mana_system_hud.gui
loc_roots:
  - localization/english/ancient_magic_game_concepts_l_english.yml
  - localization/english/magic_gui_l_english.yml
  - localization/english/ancient_magic_l_english.yml
trello:
---

# Mana System

> Core mana pool: hold, spend, regenerate, and sustain auras. Mana Affinity behavior and per-spell catalogs live in linked docs.

## Design

Mana is the pool a mage holds and spends to cast. Casting spends from the pool; over time the pool regenerates from Mana Generation. Higher **Magic Potential** (from Mana Affinity once trained) regenerates faster; **Mage Level** improves spell effect and advances from that potential—it is not the pool itself. Prefer these terms over “Magic Power.”

Lifecycle:

1. **Hold** — A mage stores Mana up to a personal maximum. The HUD shows current pool against that cap.
2. **Spend** — Casting (and starting an Aura) pays an upfront Mana Cost from the pool. Without enough Mana in the pool, the cast does not go through.
3. **Regen** — Each month (or each day, if the mana-generation game rule is set to daily), net Mana Generation is applied to the pool and clamped to the maximum.
4. **Aura sustain / fail** — An **Aura** is an ongoing spell that also reserves monthly Mana Generation as Mana Drain. Player-facing rule (game concepts): if the mage cannot produce enough Mana each month to sustain the Aura, **the Aura ends**. Activation also requires enough pool Mana to cast and enough monthly generation to sustain (Aura concept text).

Mana Affinity is an input to generation and potential—not owned by this page. Per-spell cast costs belong in spellbook / spell-index docs.

## Implementation

Same lifecycle as Design, with code entry points.

### 1. Hold — `var:mana` and max

- Pool variable: `var:mana`.
- Cap script value: `ancient_magic_max_mana` in `common/script_values/01_ancient_magic_mana_system_values.txt`.
- Clamp helper: `clamp_to_max_mana` (`common/script_values/00_math_utility_values.txt`) — `min = 0`, `max = root.ancient_magic_max_mana`.
- HUD bar / max label: `gui/custom_gui/mana_system_hud.gui` reads `ancient_magic_max_mana`.

**Worked max composition** (`ancient_magic_max_mana`):

- If `infinite_mana` trait → `999999`.
- Else: `ancient_magic_max_mana_perks` + (`mage_potency` × `max_mana_per_potency` [100]) + optional `max_mana_legacy_3` [100] when dynasty has `arcane_legacy_3`.
- `mage_potency` = `mana_affinity` × `0.2` (halved if `drained_of_blood_modifier`). Design name: Magic Potential; code symbol: `mage_potency`. HUD potency income row is labeled Mana Affinity (`mana_breakdown_gen_potency`).
- Note: `ancient_magic_max_mana_circles` exists as a script value but is **not** added into `ancient_magic_max_mana` today. Circle buildings *do* feed monthly gen (below).

### 2. Spend — `change_mana` and cast gates

- Shared mutate: `change_mana` in `common/scripted_effects/01_ancient_magic_effects.txt` — adds `$VALUE$` to `var:mana`, then reclamps with `clamp_to_max_mana`.
- Tooltip wrapper: `change_mana_with_tooltip`.
- Aura start pay path: `pay_aura_cost_and_get_xp_effect` (`01_ancient_magic_spell_system_effects.txt`) calls `change_mana_with_tooltip` with `$SPELL$_cost.neg`, then `change_mana_gen_tooltip` for the sustain delta, then XP.
- Aura setup: `start_aura_effect` registers target, modifier, aura bit, then `on_reset_mana_system`.
- Cast gate: `has_enough_mana_trigger` / `can_cast_aura_trigger` in `common/scripted_triggers/01_ancient_magic_spell_triggers.txt` — pool check `var:mana >= $SPELL$_cost.abs` only (no monthly-gen affordability check in that trigger).

### 3. Regen — pulse → `change_mana`

Game rule `magic_gen_freq` (`common/game_rules/01_ancient_magic_game_rules.txt`): default `am_magic_gen_monthly`; optional `am_magic_gen_daily`.

On-actions (`common/on_action/01_ancient_magic_mana_system_on_actions.txt`):

- `yearly_global_pulse` → `yearly_mana_pulse` seeds remaining ticks and starts either `monthly_mana_gen` or `daily_mana_gen` for each magus.
- **Monthly:** on day 1 of the month, `change_mana = { VALUE = ancient_magic_mana_gen }`, then decrement `mana_gen_months_left`.
- **Daily:** each day applies `(ancient_magic_mana_gen × 12) / 365` via `change_mana`, then decrements `mana_gen_days_left`.
- Mid-year join: `set_up_remaining_mana_gen_for_year`.
- Cap refresh / aura refresh hook: `on_reset_mana_system` reclamps `var:mana` and runs `update_all_auras_for_character`.

**Worked net gen** (`ancient_magic_mana_gen`):

- If `infinite_mana` → `999999`.
- Else: `ancient_magic_mana_gen_gen_only` + `ancient_magic_mana_gen_drain_only`.

Income (`ancient_magic_mana_gen_gen_only`):

| Bucket | Script value | Composition sketch |
|--------|--------------|--------------------|
| Perks | `ancient_magic_mana_gen_perks` | Perk mask bits × perk `*_gen_cost` constants (typically `+2` when unlocked) |
| Potency / affinity | `ancient_magic_mana_gen_potency` | `mana_affinity × 2` (halved if drained of blood) |
| Circles | `ancient_magic_mana_gen_circles` | owned mana-circle building tiers × `mana_gen_per_magic_circle` (`5`) |
| Legacy | `ancient_magic_mana_gen_legacy` | `+mana_gen_legacy_2` (`10`) if dynasty has `arcane_legacy_2` |

Expense (`ancient_magic_mana_gen_drain_only`):

| Bucket | Script value | Composition sketch |
|--------|--------------|--------------------|
| Auras | `ancient_magic_mana_drain_auras` | Sum of active `*_spell_gen_cost` values (negative) |
| Disease | `ancient_magic_mana_drain_disease` | `mana_devouring_*_gen_cost` (`-150`) when those traits apply |

HUD tooltip mirrors income/expense lists (`mana_gen_income` / `mana_gen_expenses` flags set in `set_up_spells`) and shows gen-only, drain-only, and net `ancient_magic_mana_gen`.

### 4. Aura sustain — `_gen_cost` drain and fail gap

- Sustain numbers live in `common/script_values/01_ancient_magic_spell_cost_values.txt` as `$SPELL$_gen_cost` / `$SPELL$_gen_cost_individual` (typically **negative**, e.g. `-15` per target).
- Those values feed `ancient_magic_mana_drain_auras` and therefore net monthly gen. Active auras reduce (or invert) regen; the pulse still applies whatever net is—there is no separate “pay sustain from pool” step beyond that net.
- **`_gen_cost` naming pitfall:** file comment in mana system values — `_gen_cost` is a misnomer. **Positive** perk constants *increase* gen; **negative** spell/aura (and disease) values *decrease* gen. UI labels still say “cost.”
- Manual stop: `stop_aura_effect` removes targets/modifiers, refunds the individual gen delta via `change_mana_gen_tooltip` with `.abs`, clears aura bit when list empty. Debug / bulk clear: `kill_aura` (does not itself reset mana system).

**Code gap vs player rule (KTD3):** Design/loc say insufficient sustain **ends** the Aura. Re-verified at write time: `monthly_mana_gen` / `daily_mana_gen` only call `change_mana`; they do **not** call `stop_aura_effect` or `kill_aura` when net gen (or pool) cannot cover sustain. `stop_aura_effect` / `kill_aura` appear on player cancel paths, debug decisions, and reset-style on_actions—not as an automatic sustain-failure shutdown. Until such wiring exists, insufficient sustain drains the pool via negative net gen / cast gates, but auras are **not** auto-cleared by the pulse.

### Entry points

| Concern | Location |
|---------|----------|
| Gen / max script values | `common/script_values/01_ancient_magic_mana_system_values.txt` |
| Aura / spell gen-cost wiring | `common/script_values/01_ancient_magic_spell_cost_values.txt` |
| Monthly / daily pulse | `common/on_action/01_ancient_magic_mana_system_on_actions.txt` |
| `change_mana` / tooltips | `common/scripted_effects/01_ancient_magic_effects.txt` |
| Aura start / pay / stop | `common/scripted_effects/01_ancient_magic_spell_system_effects.txt` |
| Cast / enough-mana triggers | `common/scripted_triggers/01_ancient_magic_spell_triggers.txt` |
| Pulse game rule | `common/game_rules/01_ancient_magic_game_rules.txt` (`magic_gen_freq`) |
| HUD breakdown SGUIs | `common/scripted_guis/01_ancient_magic_hud_sguis.txt` |
| HUD GUI | `gui/custom_gui/mana_system_hud.gui` |

## Related docs

- [spellbook-and-auras.md](spellbook-and-auras.md)
- [advancement.md](advancement.md)
- [mana-affinity-interactions.md](../event-chains/mana-affinity-interactions.md)
- [spell-index.md](../systems/spell-index.md)
- [gui-and-hud.md](../systems/gui-and-hud.md)
