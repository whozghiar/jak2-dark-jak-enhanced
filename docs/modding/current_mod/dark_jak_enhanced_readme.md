# Dark Jak Enhanced Mod Readme (Jak 2)

> - **Branch:** `jak2/features/dark_jak_enhanced`
> - **Game:** Jak II
> - **Status:** Operational (AI-assisted)

---

## 1. Description & Features

The **Dark Jak Enhanced** mod adds a full 3rd evolutionary stage to Dark Jak in Jak 2: the **Mega-Mega Dark Jak (Titan / Colossus)**, alongside critical quality-of-life, acrobatic, and responsiveness enhancements:

1. **Progressive 3-Tier Transformation (via `L2`):**
   - **1st `L2` Press (Normal Jak):** Transforms into **Classic Dark Jak** (scale x1.05).
   - **2nd `L2` Press (Dark Jak):** Evolves into **Mega Dark Jak / Dark Giant** (scale x2.0).
   - **3rd `L2` Press (Mega Dark Jak):** Evolves into **Mega-Mega Dark Jak / Titan** (scale x3.5).
   - *Note:* Progressive evolution is fully unlocked in standard gameplay without requiring story cheat unlocks.
2. **Instant Manual De-Transformation (`R2`):**
   - Press **`R2`** at any moment in any state to instantly revert back to normal Jak.
   - **100% Dark Eco Consumption:** All collected dark eco is consumed upon transformation, and zeroed out when exiting Dark Jak (regardless of whether by timer, `R2` cancel, Dark Bomb, Dark Blast, or death).
3. **Pristine Original HUD:**
   - The HUD remains 100% authentic and unmodified.
4. **Dynamic Panoramic Camera:**
   - Smoothly adjusts camera distance and height (`string-min-length 3.2`, `string-max-length 2.8`) to comfortably frame the colossus.
5. **Heavy Footsteps & Seismic Trample:**
   - Double screen-shake intensity on heavy footfalls during walking/running.
6. **Instant Dark Bomb Activation:**
   - Allows instant triggering of the Dark Bomb at any moment during jump ascent or descent on Square press, cancelling upward momentum immediately for a fast, responsive plunge.
7. **Robust Dark Blast (No Surface Abort):**
   - Dark Blast barrage no longer prematurely cancels back to normal Jak when in confined spaces, low ceilings, or touching obstacles.
8. **Unlocked Roll & Roll-Flip for Level 1 Dark Jak:**
   - Restored rolling (`L1` while moving) and roll-flip jump (`L1 + X`) exclusively for **Level 1 Dark Jak**, while preserving the heavy brute locomotion (no rolling) for the colossal **Mega Dark Jak (Giant / Mega-Giant)** stages.

---

## 2. Technical Architecture & Modifications

| File | Subsystem | Modifications |
| :--- | :--- | :--- |
| [`goal_src/jak2/engine/target/target-h.gc`](../../../goal_src/jak2/engine/target/target-h.gc) | Type Definitions | Added `(mega-giant)` to the `darkjak-stage` bitfield enum. |
| [`goal_src/jak2/engine/target/target-util.gc`](../../../goal_src/jak2/engine/target/target-util.gc) | Target Utilities | In `can-roll?`, allow rolling for Level 1 Dark Jak while disabling it for the `giant` and `mega-giant` stages. |
| [`goal_src/jak2/engine/target/target-darkjak.gc`](../../../goal_src/jak2/engine/target/target-darkjak.gc) | Dark Jak States | Unlocked progressive evolution in `want-to-darkjak?`, `R2` manual cancel in `target-darkjak-post`, instant upward-momentum cancellation in `target-darkjak-bomb0`, collision-resilient `:trans`/`:exit` in `target-darkjak-bomb1`, and 100% dark eco drain on transformation exit in `target-darkjak-end-mode`. |
| [`goal_src/jak2/engine/target/target-handler.gc`](../../../goal_src/jak2/engine/target/target-handler.gc) | Event Handlers | Doubled camera smush intensity on `effect-control` footsteps in the `mega-giant` stage. |
| [`goal_src/jak2/engine/target/target.gc`](../../../goal_src/jak2/engine/target/target.gc) | Player Locomotion | Scaled jump velocity thresholds, instant Dark Bomb triggers, and support for roll-flip jumps in Level 1 Dark Jak. |

| [`goal_src/jak2/pc/debug/dark-jak-enhanced-menu.gc`](../../../goal_src/jak2/pc/debug/dark-jak-enhanced-menu.gc) *(new)* | Debug Menu | Registers `Debug ▸ Mods ▸ dark-jak-enhanced ▸ Enable`. Resident in GAME.CGO. |
| [`goal_src/jak2/dgos/game.gd`](../../../goal_src/jak2/dgos/game.gd) | DGO layout | Adds `dark-jak-enhanced-menu.o` right after `mods-menu.o`. |

**Runtime toggle (mandatory procedure):** every behaviour above is gated behind `*mod-dark-jak-enhanced-enable*` (`define-perm` in `target-h.gc`, `#f` by default). With it OFF the player is byte-for-byte stock Dark Jak — single story-gated Giant, proportional eco retention, no `R2` cancel, no rolling, vanilla Dark Bomb / Dark Blast. `target-darkjak-giant` itself needs no flag check: its Mega-Giant path is only reachable through the modded `want-to-darkjak?` branch, so gating that closes it.

**Reused engine systems (no new engine code):** the mod rides the existing `darkjak-stage` bitfield, `target-darkjak` state machine, and `effect-control` footstep hooks — it only extends the enum, re-tunes scaling/velocity constants, and adds override branches. No new types, DGOs, or art groups.

---

## 3. How to Test & Play

1. Start the game via the REPL or the boot command:
   ```bash
   task boot-game
   ```
2. Collect dark eco pills or enable debug cheat mode in the REPL:
   ```lisp
   (set! (-> *setting-control* user-default cheat-mode) 'debug)
   ```
3. **Enable the mod** (OFF by default — non-regression rule): open the debug menu →
   **`Debug ▸ Mods ▸ dark-jak-enhanced ▸ Enable`**. With it off, Dark Jak behaves
   exactly like stock Jak 2. The choice survives level reloads (`define-perm`).
4. Press **`L2`** to transform:
   - **`R2`:** instantly reverts Jak to his normal form at any moment.
   - **2nd & 3rd `L2`:** evolve into Mega Dark Jak and Titan (3.5x).
   - **`L1` / `L1 + X`:** roll and roll-flip in Level 1 Dark Jak.
   - **Square in air (Dark Bomb):** instant Dark Bomb plunge.
   - **L1 + Square (Dark Blast):** collision-resilient Dark Blast.

---

## 4. Current Status & Investigations

- **Stable / working as intended:** the 3-tier `L2` evolution, `R2` universal cancel, 100% eco drain on exit, panoramic camera, doubled footstep shake, instant Dark Bomb, no-abort Dark Blast, and Level-1-only roll all behave as designed in-game.
- **HUD deliberately untouched:** an earlier iteration added a purple HUD timer bar and eco meter; it was reverted (`47cf701a0`) to keep the HUD 100% authentic. The eco-drain and manual-cancel behavior is now driven entirely from `target-darkjak.gc`, no HUD code.
- **Not yet investigated:** whether the 3.5x Titan collision sphere clips through low geometry in tight interiors, and whether the `mega-giant` footstep camera shake should ease out rather than being a flat 2x multiplier.

---

## 5. Modding Changes Log

| Date | Touched/Created Files | Technical Description | Objective |
| :--- | :--- | :--- | :--- |
| 2026-08-22 | `target-h.gc`, `target-darkjak.gc`, `target-handler.gc` | Added the `(mega-giant)` enum bitfield and implemented 3.5x progressive scaling, panoramic camera, and amplified footstep shakes. | Implement Stage 3 Colossal Dark Jak. |
| 2026-08-22 | `target.gc` | Scaled the vertical jump velocity gates by `darkjak-giant-interp`. | Fix Dark Bomb not triggering in Mega Giant mode. |
| 2026-08-22 | `target-darkjak.gc` | Removed the `on-surface` abort in `target-darkjak-bomb1 :trans` and cleaned `:exit`. | Prevent Dark Blast from cancelling prematurely on surface contact. |
| 2026-08-22 | `target.gc`, `target-darkjak.gc` | Allowed an instantaneous Square-press trigger during jumps and zeroed upward `transv` on `bomb0 :enter`. | Allow a responsive instant plunge for Dark Bomb. |
| 2026-08-22 | `target-util.gc`, `target.gc` | Enabled roll only for Level 1 Dark Jak in `can-roll?` and disabled it for the `giant` and `mega-giant` stages. | Keep an agile roll for Level 1 Dark Jak while keeping the giant stages heavy and grounded. |
| 2026-08-22 | `target-darkjak.gc` | Added the `R2` manual de-transformation hook and ensured 100% dark eco consumption when the transformation ends. | Provide a universal `R2` cancel and standard eco consumption without HUD modification. |
| 2026-08-30 | `docs/modding/current_mod/dark_jak_enhanced_readme.md` | Added the full French version of the changelog, added the "Current Status & Investigations" section (both languages), and replaced the `c:/Users/...` absolute file links with repo-relative paths. | Bring the mod readme into compliance with the modding directive. |
| 2026-09-08 | `goal_src/jak2/pc/debug/dark-jak-enhanced-menu.gc` *(new)*, `goal_src/jak2/dgos/game.gd`, `target-h.gc`, `target-darkjak.gc`, `target-handler.gc`, `target-util.gc`, `target.gc` | **Runtime on/off via the unified Debug ▸ Mods tab.** Added `*mod-dark-jak-enhanced-enable*` (`define-perm` in `target-h.gc`, `#f` default). New resident GAME.CGO menu file registers `(mods-menu-register "dark-jak-enhanced" …)`, wired into `game.gd` after `mods-menu.o`. Gated every behaviour delta to a stock fallback: `want-to-darkjak?` (stock story-gated single Giant), `target-darkjak-end-mode` / `bomb0` / `bomb1` eco drains, `target-darkjak-process` R2 cancel, `bomb0` instant-plunge velocity kill, `bomb1` `:exit`/`:trans` (stock ground/slide aborts restored), `bomb0` `sv-20` and the Dark Blast reach (stock 614400.0), `can-roll?` (stock: no roll for any Dark Jak), the 3 instant-Dark-Bomb square triggers in `target.gc` (stock velocity window), the roll / roll-flip `darkjak-giant-interp` scaling, and the footstep camera shake. `target-darkjak-giant` needs no gate — its Mega-Giant branch is dead once `want-to-darkjak?` is stock-gated. | Comply with CLAUDE.md golden rules #2/#3: mod OFF by default, switchable at runtime from Debug ▸ Mods, without editing `default-menu*.gc`. |
