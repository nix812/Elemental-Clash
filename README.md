# Elemental Clash

A browser-based top-down hero battle game built entirely in HTML, CSS, and JavaScript. Choose your elemental hero, master their abilities, and clash against bots across dynamic arenas.

---

##  How to Play

1. Download or clone this repository
2. Open `elemental-clash.html` in any modern browser
3. No installation, server, or dependencies required — runs entirely in the browser

---

## Hero Roster

Ten heroes, each with a unique element, combat class, 3 abilities, a special, and a passive:

| Hero | Element | Class |
|------|---------|-------|
| EMBER | Fire | Ranged |
| TIDE | Water | Hybrid |
| STONE | Earth | Melee |
| GALE | Wind | Hybrid |
| VOID | Shadow | Hybrid |
| MYST | Arcane | Ranged |
| VOLT | Lightning | Ranged |
| FROST | Ice | Ranged |
| FORGE | Metal | Melee |
| FLORA | Nature | Melee |

---

##  Features

- **10 Unique Heroes** — each with 3 abilities, a special, and a passive ability
- **Elemental Interactions** — elements counter and synergise with each other
- **Combat Classes** — Melee, Ranged, and Hybrid playstyles
- **Hero Passives** — passive abilities that reward skilled play and build around hero identity
- **Shrinking Arena** — dynamic zone that forces conflict as matches progress
- **Weather Zones** — weather events that buff certain heroes and alter abilities
- **Bot Opponents** — play solo vs AI opponents
- **Gamepad Support** — fully controller compatible with remapped diamond button layout
- **Keyboard & Mouse** — fully supported with remappable bindings
- **Mobile Friendly** — responsive UI with touch joystick controls
- **Element Roster** — in-game hero viewer with full stats, abilities, and lore

---

##  Controls

| Action | Keyboard/Mouse | Gamepad |
|--------|---------------|---------|
| Move | WASD / Joystick | Left stick |
| Ability 1 | Q | X / Square |
| Ability 2 | E | A / Cross |
| Ultimate | R | Y / Triangle |
| Special | F | B / Circle |
| Sprint | Shift | Bumper |

> All bindings are remappable in **Options → Controls**

---

##  Combat System

- Each hero has **3 abilities** + a **special** (free, on its own cooldown)
- Abilities cost **mana**, which regenerates passively
- Landing an ability opens a **0.5s window** for a **+20% damage** bonus on your next auto-attack
- Hitting a CC'd target (stunned, rooted, frozen) deals **+30% bonus damage**
- Each hero has a **unique passive** that rewards building around their kit

---

## Project Structure

```
elemental-clash/
├── elemental-clash.html   # Entire game — self-contained single file
└── README.md
```

---

## Built With

- Vanilla HTML, CSS, and JavaScript — no frameworks, no build step, no dependencies

---

## In Development

- Expanded weather event system
- Additional heroes and elemental types
- Multiplayer / ranked play
- UI polish

---
## Author

Built by Nic — an independent game design project in active development.
