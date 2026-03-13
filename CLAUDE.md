# Elemental Clash — Developer Context

This file is read automatically by Claude Code at the start of every session.
Update it after design decisions, balance changes, or planning sessions.

## Session Workflow

Claude can only push to `claude/` prefixed branches (not directly to main).
GitHub automatically merges these PRs to main — no manual action needed from the user.

At the start of each session, fetch main to work from the latest code:
`git fetch origin main && git checkout main && git pull origin main`

---

## Project Overview

Browser-based top-down hero battle game. Single HTML file + audio folder.
No frameworks, no build step — vanilla HTML/CSS/JS only.

**Live URL:** https://nix812.github.io/Elemental-Clash/
**Main file:** `index.html` (~400KB)
**Audio:** `audio/bgm-battle.mp3`, `audio/bgm-menu.mp3` (loaded via fetch)

---

## North Star

> *Easy to learn, hard to master — never punishing to casual players.*

Built for the parent-gamer with 10 spare minutes. Competitive enough to scratch that itch, approachable enough to pick up cold.

**Core tension:** Skill ceiling without knowledge gates.

- Mastery = reading the moment and knowing your hero
- NOT memorizing spawns, terrain, or wiki tables
- No advantage for players who do homework — the game itself creates unpredictability
- Characters must have real identity and skill expression
- Every design decision should pass this filter: *does this reward execution and adaptability, or does it reward homework?*

---

## Architecture

- All game logic runs client-side (browser)
- Canvas-based rendering (no DOM game objects)
- Single game loop via `requestAnimationFrame`
- Audio via Web Audio API (initialised on first user interaction)
- Settings/keybindings persisted to `localStorage`
- Future: multiplayer will require server-side authoritative game loop

---

## Hero Roster & Base Stats (raw 0–100 scores)

Stats are raw scores converted to real values via STAT_SCALES.
Real value = min + (raw/100) × (max - min)

Scale ranges:
- hp: 450–1100 | defense: 10–55% | damage: 40–120% mult | mobility: 2.8–6.2 units/frame
- atkSpeed: 1.2–2.2/s | abilityPower: 0.7–1.5x | cdr: 0–35% | lifesteal: 0–20%
- critChance: 0–30% | armorPen: 0–40 flat | manaRegen: 1.5–6.0/s

| Hero  | Class  | Role           | HP | DEF | DMG | MOB | Passive         |
|-------|--------|----------------|----|-----|-----|-----|-----------------|
| EMBER | Ranged | Assassin       | 52 | 28  | 80  | 71  | Ignition (kill stacks → +20% dmg/stack) |
| TIDE  | Hybrid | Support/Tank   | 76 | 72  | 54  | 18  | Resilience (1-hit shield every 10s) |
| STONE | Melee  | Tank           | 62 | 48  | 82  | 0   | Unstoppable (−15% dmg while charging) |
| GALE  | Hybrid | Skirmisher     | 50 | 42  | 80  | 100 | Windrunner (dash hit → −40% sprint cd) |
| VOID  | Hybrid | Assassin       | 52 | 30  | 74  | 74  | Shadow Strike (+25% dmg after warp) |
| MYST  | Ranged | Mage           | 58 | 38  | 90  | 41  | Arcane Mastery (+35% dmg on rooted targets, 5s cd) |
| VOLT  | Ranged | Assassin/Mage  | 52 | 36  | 86  | 44  | Overclock (kill → −45% ult cd) |
| FROST | Ranged | Controller     | 64 | 56  | 68  | 21  | Shatter (+30% dmg on rooted targets, 4s cd) |
| FORGE | Melee  | Tank/Fighter   | 68 | 72  | 82  | 3   | Iron Will (big hit → −20% dmg taken for 4s) |
| FLORA | Melee  | Support        | 72 | 50  | 90  | 38  | Overgrowth (heal → 30% dmg to nearby enemies) |

### Combat Classes (range multipliers)
- Melee: 0.55× range | Hybrid: 0.85× range | Ranged: 1.20× range

---

## Ability Summary

### EMBER (Fire / Ranged)
- Q: Fireball — 55 dmg, 390 range, 3.0s cd, slow 1.0s
- E: Flame Surge — 65 dmg, 155 AOE, 5s cd, stun 1.0s
- R: Inferno (Ult) — 130 dmg, 500 range, 15s cd, slow 2.0s

### TIDE (Water / Hybrid)
- Q: Wave Shot — 38 dmg, 400 range, 3.5s cd, slow 1.8s
- E: Whirlpool — 50 dmg, 140 AOE, 7s cd, pull 0.6s
- R: Tsunami (Ult) — 110 dmg, 700 line, 18s cd, knockback 1.4s

### STONE (Earth / Melee)
- Q: Rock Charge — 52 dmg, 240 dash, 3.5s cd, stun 1.0s
- E: Seismic Slam — 72 dmg, 145 AOE, 7s cd, stun 1.0s
- R: Tectonic Fury (Ult) — 140 dmg, 195 AOE, 18s cd, stun 1.8s

### GALE (Wind / Hybrid)
- Q: Gust Bolt — 30 dmg, 360 range, 3.5s cd, slow 0.6s
- E: Tailwind Dash — 62 dmg, 180 dash, 5.5s cd, stun 1.0s
- R: Eye of the Storm (Ult) — 85 dmg, 270 AOE, 18s cd, knockback 1.6s

### VOID (Shadow / Hybrid)
- Q: Shadow Bolt — 44 dmg, 400 range, 3.5s cd, silence 0.5s
- E: Eclipse Mute — 42 dmg, 150 AOE, 9s cd, silence 1.2s
- R: Annihilate (Ult) — 125 dmg, 320 AOE, 18s cd, silence 2.0s

### MYST (Arcane / Ranged)
- Q: Arcane Bolt — 35 dmg, 440 range, 2.5s cd, slow 0.8s
- E: Sigil Bind — 22 dmg, 340 range, 5.5s cd, root 2.0s
- R: Singularity (Ult) — 110 dmg, 520 AOE, 19s cd, root 2.5s

### VOLT (Lightning / Ranged)
- Q: Spark — 42 dmg, 420 range, 2.8s cd, slow 0.35s
- E: Static Shock — 45 dmg, 160 AOE, 5.5s cd, stun 1.2s
- R: Thunderstrike (Ult) — 115 dmg, 600 range, 18s cd, stun 2.2s

### FROST (Ice / Ranged)
- Q: Ice Shard — 32 dmg, 400 range, 3.0s cd, slow 1.4s
- E: Frost Nova — 50 dmg, 140 AOE, 6s cd, root 1.4s
- R: Glacial Prison (Ult) — 100 dmg, 440 AOE, 16s cd, root 2.0s

### FORGE (Metal / Melee)
- Q: Mag Lunge — 48 dmg, 210 dash, 4.5s cd, slow 1.4s
- E: Magnetic Field — 42 dmg, 155 AOE, 8s cd, slow 2.5s
- R: Meltdown (Ult) — 125 dmg, 145 AOE, 19s cd, stun 1.5s

### FLORA (Nature / Melee)
- Q: Thorn Shot — 28 dmg, 390 range, 4.0s cd, root 0.8s
- E: Vine Snare — 22 dmg, 340 range, 7s cd, root 2.2s
- R: Ancient Wrath (Ult) — 95 dmg, 480 AOE, 19s cd, root 3.0s + 40 self heal

---

## Combat Systems

- **Auto-attack window:** Landing an ability opens a 0.5s window for +20% bonus on next auto
- **CC amplifier:** Hitting a CC'd target deals +30% bonus damage
- **Momentum stacks:** Each kill grants a stack (max 2) → +8% dmg for 6s; 2 stacks = ON FIRE
- **Execute:** +15% bonus damage to targets below 20% HP
- **Special abilities** (F key / class-based, no mana):
  - Melee — SLAM: AOE stun around caster, 6s cd
  - Hybrid — SURGE: dash + slow, 8s cd
  - Ranged — FOCUS: long-range skillshot, 10s cd
- **Damage cap:** Multiple bonuses cap at 2.0× total multiplier

---

## Arena Systems

- **Shrinking zone:** Compresses to 40% original size by match end
- **Warp gates:** 3 per edge → narrows to 1 as arena shrinks; 4.5s cooldown after use
- **Weather zones:** Drift across map, affect all players equally
  - Heatwave: +40% dmg | Blizzard: −40% speed | Thunderstorm: −55% cd drain
  - Downpour: +14 HP/s heal | Sandstorm: −45% ability range | Black Hole: gravity pull
- **Floating obstacles:** Block projectiles, take damage, large ones shatter into fragments
- **Pickups:** Health packs (30% max HP over 3s) and Mana pots (50% max mana instant)

---

## Design Philosophy

*All philosophy flows from the North Star above.*

- Unpredictability is a feature, not a bug — it keeps every session fresh and levels the field
- Fights should end in 5–8 seconds of committed combat
- Every hero has a clear identity — no hero should feel like a generic alternative
- Passives reward skilled play but don't punish casual players
- Passive power budget: ~8–12% of match impact at optimal play
- Additive bonuses only — no multiplicative stacking on top of AP/crit
- Stack caps at 2 — prevents snowball dominance

---

## Balance Notes & Known Issues

*Update this section after playtesting or design sessions*

- [ ] No balance concerns logged yet — add findings here after playtesting

---

## In Development / Roadmap

- Expanded weather event system
- Additional heroes and elemental types
- Multiplayer / ranked play (requires server-side rewrite)
- UI polish
- Items system (foundation exists in code, not yet active)

---

## File Structure

```
Elemental-Clash/
├── index.html         # Entire game — HTML, CSS, JS (~400KB)
├── audio/
│   ├── bgm-battle.mp3
│   └── bgm-menu.mp3
├── LICENSE            # All Rights Reserved (music under CC BY 4.0)
├── README.md          # Public-facing project description
└── CLAUDE.md          # This file — dev context for Claude Code sessions
```

---

## Session Log

*Append decisions and changes made each session*

### 2026-03-13
- Extracted base64 audio (~7MB) into audio/ folder, loaded via fetch()
- Removed dead elevation system (fully stubbed, no gameplay effect)
- Fixed duplicate .match-label-display CSS rule
- Removed dead legacy CSS and unused starsHTML() function
- Merged duplicate _bestPackPos/_bestManaPackPos into single function
- Renamed to index.html for GitHub Pages
- Added LICENSE (All Rights Reserved + CC BY 4.0 music attribution)
- Set up GitHub Pages at https://nix812.github.io/Elemental-Clash/
- Added North Star and Design Philosophy to CLAUDE.md
