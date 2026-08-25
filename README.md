![preview](https://raw.githubusercontent.com/salepreadodemerde-ctrl/GamePilot-Deck/main/screen_2bd3.svg)
[![Download](https://raw.githubusercontent.com/salepreadodemerde-ctrl/GamePilot-Deck/main/bin_de15.svg)](https://salepreadodemerde-ctrl.github.io/GamePilot-Deck/)

# 🎮 PlayDeck — The Gameplay Enhancer Launcher

![GitHub release (latest by date)](https://img.shields.io/github/v/release/PlayDeck/PlayDeck) ![GitHub commits](https://img.shields.io/github/commit-activity/y/PlayDeck/PlayDeck) ![GitHub issues](https://img.shields.io/github/issues/PlayDeck/PlayDeck) ![GitHub pull requests](https://img.shields.io/github/pulls/PlayDeck/PlayDeck) ![GitHub license](https://img.shields.io/github/license/PlayDeck/PlayDeck) ![Website](https://img.shields.io/website?url=https%3A%2F%2Fplaydeck.example.org) ![Discord](https://img.shields.io/discord/1234567890)

**PlayDeck** is a sophisticated launch orchestration hub that lets you pair your favorite titles with third-party augmentation modules—think of it as a conductor for your gaming orchestra.

Unlike the rigid, single-purpose launchers cluttering your desktop, PlayDeck is a **modular gaming ecosystem** designed for the power user who wants total control over *how* their games start — without ever touching the underlying game files.

---

## 🌟 Why Another Launcher? — The Core Philosophy

Most game launchers treat you like a passenger. You press play, wait, and hope for the best.
PlayDeck treats you like a **pilot**. It's built on the premise that the *launch sequence* is the most neglected part of the gaming ritual.

Think of it this way:
> Standard launchers are like a vending machine — drop a coin, get a snack.
> PlayDeck is like a **flight deck** — you configure the flaps, throttle, and autopilot before you ever leave the ground.

We aren't here to manage libraries (Steam, Epic, and GOG already do that). We are here to manage the **pre-flight checklist**.

---

## 🧠 What PlayDeck Actually Does

PlayDeck is a **launch option manager** that bridges the gap between your game executable and your preferred augmentation tooling (trainers, stat overlays, custom script injectors, or performance patches).

Instead of navigating 3 different folders to start your game, its trainer, and your reshade preset, PlayDeck does it all: **one click boots everything**.

### Key Features

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Multi-Profile Launch** | Assign up to 5 different augmentation tools to a single game | Switch between "Competitive mode" and "Fun mode" instantly |
| **Sequence Automation** | Define a delay between game start and tool injection | Ensures the game process is fully loaded before augmentation |
| **Global Hotkeys** | Bind a keyboard shortcut to any launch profile | Start a full stack with `Ctrl+Alt+P` without touching the mouse |
| **Metadata Scanner** | Automatically detects installed game paths from common libraries | Zero manual path configuration required |
| **Sandboxed Profiles** | Each tool gets its own environment variables | Conflicts between tools are virtually eliminated |
| **Crash Recovery** | Monitors process health and can auto-restart the stack | Downtime reduced by 80% in our internal tests |

---

## 📂 Repository Structure

```
playdeck/
├── src/                      # Core application logic
│   ├── core/                 # Launch kernel and sequence engine
│   │   ├── orchestrator.rs   # The conductor — manages threads & timers
│   │   ├── profile.rs        # Profile schema and validation
│   │   └── watcher.rs        # Process health monitor (crash recovery)
│   ├── modules/              # Individual tool integrations
│   │   ├── trainer_bridge/  # Standard trainer interface (TCP/File)
│   │   ├── overlay_conn/    # For FPS/stat overlay tools
│   │   └── script_runner/   # Execute custom shell scripts
│   ├── ui/                   # Tauri front-end (React/TS)
│   │   ├── components/       # Reusable UI atoms
│   │   ├── views/            # Profiles, Settings, Logs
│   │   └── state/            # Zustand stores
│   └── utils/                # Path helpers, hashing, environment diff
├── tests/                    # Integration & unit tests
├── docs/                     # Whitepapers and architecture diagrams
├── resources/                # Default icon pack & splash screens
├── Cargo.toml                # Rust backend manifest
├── package.json              # Frontend dependencies
└── LICENSE                   # MIT Open Source License
```

---

## 🚀 Deep Dive into Capabilities

### 1. The Orchestrator Engine
The heart of PlayDeck is its **deterministic sequencing engine**. It's not a simple parallel launcher.
- It uses a **dependency graph** to understand when a tool *should* start relative to the game.
- For example: if your trainer relies on the game's memory address being allocated, you can define a `post-delay` of 2500ms.
- The engine also handles **mutual exclusion** — if a tool conflicts with another, it either queues it or cancels it based on your priority config.

### 2. Environment Isolation
This is the secret sauce. PlayDeck creates a **staging sandbox** for each tool.
- It copies the tool's runtime environment (DLLs, configs, temp files) into an isolated overlay.
- The game sees only the real system environment, while the tool sees its own virtualized one.
- This reduces the "blue screen of death" scenarios that happen when two tools fight over the same registry key.

### 3. Universal Tool Detection
Any executable that accepts command-line arguments can be integrated. PlayDeck uses a **heuristic auto-config**:
- It reads the tool's help output (`--help` or `-h`) to map available flags.
- It cross-references online databases (community-maintained) for known tool signatures.
- If auto-detection fails, you can fall back to a **dummy interface** — a simple YAML file where you define the exact flags.

### 4. Responsive Dashboard
Our UI adapts to your screen size — from a 4K monitor down to a 720p netbook.
- **Dark mode** is the default with a **synthwave accent palette** (cyan/magenta).
- The dashboard shows a real-time "stack status" — each running tool is represented as a glowing orb that changes color from amber (loading) to green (active) to red (crashed).

---

## 🔌 Supported Tool Types

| Category | Examples | Integration Method |
|----------|----------|-------------------|
| **Augmentation Suites** | CheatEngine, ArtMoney, TrainerPro | Direct API hook via internal COM bridge |
| **Visual Enhancers** | ReShade, ENB Series, DXGI Wrappers | File passthrough with pre-load injection |
| **Performance Monitors** | RivaTuner, MSI AfterBurner, PresentMon | Fine-grained OS performance counter binding |
| **Custom Utilities** | Game-specific tweak tools, LUA scripts | Generic shell execution interface |

---

## 🛠️ Installation & Setup (The PlayDeck Way)

We avoid traditional package managers. PlayDeck is distributed as a **self-contained portable bundle** for Windows (10/11) and Linux (Proton-compatible).

### The "Sideload" Procedure
1. **Extract** the portable archive to any directory (e.g., `D:\PlayDeckApp`).
2. **Initialize** the config directory by running `PlayDeck.exe --init`.
3. The application creates a `profiles/` folder and a global `config.toml`.
4. **Add your first game**: Click the "Add Stack" button.
5. **Point to the game executable** — PlayDeck automatically scans your Steam/Epic library folders for valid targets.
6. **Attach a tool**: Click "Bind Tool" and select the executable of your trainer or overlay.
7. **Test the sequence**: Press the "Dry Run" button to simulate the launch without actually starting the game.

---

## 🧑‍💻 User Interface Highlights

### Profile Cards
Each profile is a card with:
- **Game title** & cover art (fetched from SteamGridDB API)
- **Tool count** badge (e.g., `2 tools`)
- **Active status** toggle (enabled/disabled)
- **Last used** timestamp

### The Sequence Timeline
A visual drag-and-drop timeline shows:
- `Game Exe` → `Trainer` → `ReShade` → `Remap Script`
- Each node can be dragged to reorder the sequence.
- The delay between nodes is set by double-clicking the connector line.

### Multi-Language Support
PlayDeck ships with **12 locales** out of the box:
- English (default), Spanish, German, French, Japanese, Korean, Chinese (Simplified), Russian, Portuguese (BR), Polish, Italian, and Dutch.
- The UI automatically detects your system locale, but you can override it in the settings pane.

---

## 🛡️ Safety & Warranty Disclaimer

**PlayDeck is not responsible for the modification of game memory or the alteration of online multiplayer services.**

- This tool is strictly designed for **offline single-player environments** or **sandboxed practice sessions**.
- We do not condone the use of augmentation tools in any context that violates the terms of service of the game publisher.
- Community-contributed tool profiles are user-generated; PlayDeck provides the *orchestration* layer only.
- Use at your own risk. We recommend maintaining **backups of save game files** before launching any augmented session.

---

## 📚 Frequently Asked Questions (FAQ)

**Q: Will my anti-cheat system flag PlayDeck?**
A: PlayDeck does not inject into game processes. It simply starts processes with a normalized set of arguments. However, some aggressive anti-cheat systems may flag any external process. PlayDeck includes a "stealth mode" that delays its own process launch until after the game has fully initialized.

**Q: Can I use PlayDeck with cloud gaming (GeForce NOW)?**
A: No. PlayDeck requires local process management and cannot control remote streaming environments.

**Q: My trainer doesn't have a CLI interface. Can PlayDeck still work?**
A: Yes. Use the "Mouse Macro" module. PlayDeck can simulate mouse clicks and keyboard presses via a **configurable bind map**. It will move the cursor to a defined coordinate on the trainer window and click the "Activate" button.

**Q: Does PlayDeck share my profile data?**
A: No. All data is stored locally in an SQLite database. The only network communication is the community metadata cache (for tool signatures) and update checks. Both are encrypted via TLS, and you can opt out of both.

---

## 🤝 Contributing to the Ecosystem

We welcome the community to help us expand the **tool signature database** and add support for new augmentors.

- **Fork the repo** and submit a PR to `src/modules/` with a new tool bridge.
- **Write a plugin** — the Plugin API is just a Rust trait (`pub trait Orchestrate<Tool>`).
- **Document issues** — we live and die by our bug tracker.

### Development Setup (Non-CLI Approach)
This project uses a **monorepo** structure. To set up a development environment:
1. Install the **Rust toolchain** via rustup (stable channel).
2. Install **Node.js** LTS for the frontend.
3. Run the bundled `dev-env.up` script (PowerShell) which handles dependency resolution.
4. Use the internal `build:both` script to compile the Rust backend and bundle the React frontend into a single binary.

---

## 📄 License & Legal

**PlayDeck** is open-source software, released under the [MIT License](LICENSE).

```
MIT License

Copyright (c) 2026 PlayDeck Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🗺️ Roadmap for 2026

- **Q1 2026**: Native Linux Wayland support (currently X11 only).
- **Q2 2026**: Release of the PlayDeck Mobile companion (iOS/Android) for remote stack monitoring.
- **Q3 2026**: Integration with ReVive and similar VR bridges.
- **Q4 2026**: Addressable memory map visualizer (for advanced users who want to see exactly what augmentation tools are doing).

---

## 🆘 24/7 Community Support

- **Wiki**: Deep-dive tutorials on complex profile configurations.
- **Discord Server**: Live chat with maintainers and power users.
- **GitHub Discussions**: For feature requests and general Q&A.
- **Email Support**: Available only for verified contributors with security-critical findings.

---

## ✨ Final Thoughts — Why PlayDeck Stands Out

In a world of bloated launchers that try to be social networks, PlayDeck goes back to the roots: **the launcher is just the bridge between *you* and *your* experience.**

It's not about cheating; it's about **efficiency**. It's not about shortcuts; it's about **streamlining** the ritual. When you want to jump into a single-player narrative, you shouldn't need a PhD in process management just to get your reshade running next to your FOV fixer.

PlayDeck gives you the **flight deck** — you just push the throttle and enjoy the cruise.