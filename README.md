# Rider HUD

[![Minecraft](https://img.shields.io/badge/Minecraft-26.3-brightgreen.svg)](https://playhorizon.dev)
[![Horizon Native](https://img.shields.io/badge/Horizon-Native_Mod-orange.svg)](https://playhorizon.dev/docs/mod-code)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)

A high-performance, lightweight native Horizon mod for **Minecraft 26.3** that enhances the riding HUD experience by simultaneously displaying mount health, player hunger, and smart jump/experience bars without visual overlap.

---

## Features

- **Simultaneous Mount Health & Player Hunger:**
  - In vanilla Minecraft, mounting a horse replaces your hunger bar with mount health hearts.
  - **Rider HUD** keeps your hunger bar visible in its standard place above the hotbar and displays mount health hearts on the row directly above it, so neither indicator is hidden.
- **Dynamic Non-Overlapping Layout:**
  - **Player Health & Absorption:** Standard position on the left.
  - **Player Armor:** Stacks above player health on the left.
  - **Player Hunger:** Standard position on the right above the hotbar.
  - **Mount Health:** Displayed one row above the hunger bar on the right (stacking additional rows upward for high-health mounts).
  - **Air Bubbles:** When submerged, bubbles stack cleanly above mount hearts.
- **Smart Jump & Experience Bar:**
  - During ordinary riding, your standard experience progress bar and level number remain visible.
  - When you press and hold the Jump key to charge a jump (or track camel dash cooldowns), the jump meter dynamically appears in place of the experience bar.
  - As soon as the jump completes or is canceled, the experience bar smoothly returns without arbitrary delay or polling timers.
- **Universal Vehicle Support:**
  - Fully supports horses, donkeys, mules, camels, skeleton horses, zombie horses, llamas, nautiluses, and any modded entities implementing vehicle health and jump mechanics.
  - Non-living vehicles (boats, minecarts) and non-jumping mounts seamlessly retain default vanilla behavior.
- **In-Game Configuration (Options → Mods):**
  - `Rider HUD: ON / OFF` (master toggle).
  - `Hunger While Riding: ON / OFF`.
  - `Experience While Riding: ON / OFF`.
- **Zero Runtime Overhead:**
  - 100% client-side, written in safe Rust.
  - 0 background threads, 0 polling loops, 0 unneeded heap allocations per frame.
  - Disabling the mod or dismounting immediately restores 100% vanilla behavior.

---

## Technical Hook Architecture

Rider HUD integrates cleanly with Horizon's native hook points without modifying original engine binaries:

| Hook Point | API Level | Purpose |
|------------|-----------|---------|
| `native_gui_layer_pre` | API 139 | Repositions mount health hearts above the hunger bar; dynamically swaps the contextual bar between experience and jump meter. |
| `native_health_hud` | API 27 | Renders the player hunger bar while riding and dynamically shifts air bubbles above mount hearts. |
| `native_mods` | API 1 | Registers the mod into Horizon's native mod menu list. |
| `native_settings` | API 1 | Populates configurable settings buttons in the in-game UI. |
| `native_settings_click` | API 1 | Handles interactive toggle clicks and persists user settings across game sessions. |

---

## Requirements & Compatibility

- **Minecraft Version:** `26.3`
- **Engine:** Horizon Nightly (`2ebf00a0281c592f79191e76fce0e7d522c6f425` or compatible)
- **Mixins Compiler:** `horizon-mixins` (`0d6b103c70633d8fd9443d8d0acd3aadaacedb06`)
- **Native Hooks Version:** `v1` (NATIVE_HOOKS_API >= 139)

---

## Installation

1. Download `rider_hud-0.1.0.hnmod` from the [Releases](https://github.com/maliferen/horizon-rider-hud/releases) tab.
2. Place the `.hnmod` file into your Horizon game instance directory:
   ```
   %APPDATA%\horizon\instances\<instance_name>\horizon-native-mods\
   ```
3. Launch the game through Horizon Launcher.
4. Verify the mod is active under **Options → Mods → Rider HUD**.

---

## Building from Source

### Prerequisites
- Rust stable toolchain (edition 2024 / rustc 1.95+)
- Python 3.11+

### Running Unit Tests
```bash
cargo test
cargo fmt --check
cargo clippy -- -D warnings
```

### Packaging `.hnmod`
```bash
python package.py --out builds/
```

### Official Horizon Verification
```bash
python native/tools/native.py check --horizon latest rider-hud
```

---

## Verified Release Package

- **File:** `rider_hud-0.1.0.hnmod`
- **Size:** `7,378 bytes`
- **SHA-256:** `2f8121390f87672ff023ad1933503248d19b3092b2f5a27debc8612a7f520b10`

---

## License

This project is licensed under the [MIT License](LICENSE).  
Copyright (c) 2026 maliferen.

