# Dark Jak Enhanced (Mega-Mega Dark Jak & Titan Evolution) — Jak 2

<p align="center">
  <img src="https://img.shields.io/badge/OpenGOAL-Mod-blue.svg" alt="OpenGOAL Mod">
  <img src="https://img.shields.io/badge/Game-Jak%202-orange.svg" alt="Game">
  <img src="https://img.shields.io/badge/AI--assisted-Modding-purple.svg" alt="AI Assisted">
</p>

---

> [!NOTE]
> This mod moved from the `jak2/features/dark_jak_enhanced` branch of [whozghiar/jak-project](https://github.com/whozghiar/jak-project) to this repository. Earlier releases stay installable from the launcher catalog.

## 📖 Overview
The **Dark Jak Enhanced** mod adds a full 3rd evolutionary stage to Dark Jak in Jak 2: the **Mega-Mega Dark Jak (Titan / Colossus)**, alongside critical quality-of-life improvements, restored acrobatics for Level 1 Dark Jak, instantaneous Dark Bomb activation, and enhanced collision resilience.

- **Target Game:** Jak 2
- **Repository:** [`whozghiar/jak2-mod-dark-jak-enhanced`](https://github.com/whozghiar/jak2-mod-dark-jak-enhanced)

## ✨ Key Features
- **Progressive 3-Tier Evolution (via `L2`):**
  - **1st `L2` Press (Normal Jak):** Transforms into **Classic Dark Jak** (scale x1.05).
  - **2nd `L2` Press (Dark Jak):** Evolves into **Mega Dark Jak / Dark Giant** (scale x2.0).
  - **3rd `L2` Press (Mega Dark Jak):** Evolves into **Mega-Mega Dark Jak / Titan** (scale x3.5).
  - *No story unlock required:* works directly during standard gameplay without debug cheats.
- **Manual De-Transformation (`R2`) & 100% Eco Drain:** Press `R2` at any time to immediately revert back to normal Jak. Dark eco reserve is 100% consumed upon transformation exit, regardless of cause (`R2` cancel, timer expiration, Dark Bomb, Dark Blast, or death).
- **Pristine Authentic HUD:** The original HUD is kept 100% authentic and unmodified.
- **Dynamic Panoramic Camera:** Automatic distance and height pull-back (`string-min-length 3.2`, `string-max-length 2.8`) for optimal framing of the colossal titan.
- **Heavy Footsteps & Seismic Screen-Shake:** Doubled screen-shake intensity on footfalls while walking and running as a titan.
- **Instant Dark Bomb Activation:** Pressing Square during any jump ascent or descent immediately cancels upward momentum for an instant, responsive dive plunge.
- **Collision-Resilient Dark Blast:** Dark Blast barrage no longer prematurely aborts when cast in confined interiors, under low ceilings, or touching obstacles.
- **Agile Roll & Roll-Flip for Level 1:** Restores rolling (`L1` in motion) and roll-flip jumps (`L1 + X`) exclusively for Level 1 Dark Jak, while keeping giant stages heavy and grounded.

## 🎮 Controls & Gameplay Summary
- **`L2`:** Transform / Evolve to next Dark Jak tier (Level 1 -> Mega Giant -> Titan).
- **`R2`:** Instant de-transformation back to normal Jak.
- **`L1` (in motion):** Roll (Level 1 Dark Jak only).
- **`L1 + X`:** Roll-flip jump (Level 1 Dark Jak only).
- **Square (in air):** Instant Dark Bomb plunge (cancels upward vertical momentum).
- **`L1 + Square`:** Dark Blast (stable in confined spaces).
- **`L3 + SELECT`:** Open the unified in-game Mods menu to toggle `dark-jak-enhanced` Enable/Disable (works in retail boot, no debug mode required).

## 🚀 Step-by-Step Guide to Run the Mod

### 1. Select the Active Game
Make sure your environment is targeting Jak 2:
```bash
task set-game-jak2
```

### 2. Binary Compilation
- **Status:** Layer 3 (GOAL only) — Not required if standard binaries already exist.
- **Details:** Only GOAL scripts are modified (`target-h.gc`, `target-darkjak.gc`, `target-handler.gc`, `target-util.gc`, `target.gc`). No C++ rebuild needed. For a first-time build, use:
```bash
task build-release-game
```

### 3. Asset Extraction
- **Status:** Standard extraction sufficient (once per setup).
- **Details:** Uses native in-game models, animations, and sound effects.
```bash
task extract
```

### 4. Launch the Game
Run the game natively:
```bash
task boot-game
```
*(Or iterate fast via the OpenGOAL REPL using `task repl`, then hot-reload with `(mi)` and `(r)`).*

### 5. Enable the Mod (OFF by default)
This mod ships **disabled** — a fresh install plays Dark Jak exactly like stock
Jak 2. Press **L3 + SELECT** in game to open the unified **Mods** menu (it works in a normal boot, no debug mode needed) and select:

```
Mods ▸ dark-jak-enhanced ▸ Enable
```

The choice persists across level reloads. Turn it off to restore vanilla Dark Jak
(single story-gated Giant form, no `R2` cancel, no rolling, vanilla Dark Bomb / Blast).

## 🎥 Demonstration Video
[![Demonstration Video](https://img.youtube.com/vi/eUS1cFZ_clg/maxresdefault.jpg)](https://youtu.be/eUS1cFZ_clg)

▶️ **[Watch the demonstration video on YouTube](https://youtu.be/eUS1cFZ_clg)**

## 📖 Technical Documentation
For the complete technical breakdown, architecture, and developer notes, refer to:
- 📄 [`docs/modding/current_mod/dark_jak_enhanced_readme.md`](docs/modding/current_mod/dark_jak_enhanced_readme.md)

---
*(AI-assisted)*
