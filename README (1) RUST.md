# 🦀 Rust TUI — Terminal User Interface Project

> *Built with Rust. Learned with AI. Understood by me.*

![Rust](https://img.shields.io/badge/Rust-1.75+-orange?style=flat-square&logo=rust)
![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)
![Status](https://img.shields.io/badge/status-active-brightgreen?style=flat-square)
![Methodology](https://img.shields.io/badge/methodology-AI--assisted%20learning-purple?style=flat-square)

---

## 📌 Overview

This project is a fully featured **Terminal User Interface (TUI)** application written in Rust, built on top of the [`ratatui`](https://github.com/ratatui-org/ratatui) and [`crossterm`](https://github.com/crossbeam-rs/crossbeam) ecosystem. It demonstrates real-world Rust concepts including ownership, lifetimes, traits, async I/O, and event-driven architecture — all within the constraint of a terminal renderer.

More than just a working application, this project documents a deliberate **"Learning Through AI"** methodology: using Claude as a pair programmer, a Socratic tutor, and a rubber duck — not as a shortcut, but as a structured path to genuine understanding.

---

## ✨ Features

- **Multi-panel TUI layout** — responsive grid built with `ratatui` constraints
- **Keyboard-driven navigation** — Vim-style key bindings and stateful routing
- **Real-time data rendering** — live-updating widgets (charts, tables, gauges)
- **Async event loop** — non-blocking input handling via `tokio` + `crossterm`
- **Modular architecture** — clean separation of UI, state, and data layers
- **Error handling** — idiomatic `Result`/`Option` patterns throughout

---

## 🖥️ Demo

```
┌──────────────────────────────────────────────────────────┐
│  🦀 Rust TUI Dashboard                    [q] Quit       │
├─────────────────────┬────────────────────────────────────┤
│  Navigation         │  Main Panel                        │
│  ─────────────────  │  ──────────────────────────────    │
│  > Dashboard        │   CPU  [████████░░░░░░] 54%        │
│    Metrics          │   MEM  [██████████████] 87%        │
│    Logs             │   NET  [███░░░░░░░░░░░] 21%        │
│    Settings         │                                    │
│                     │  ┌──────────────────────────────┐  │
│                     │  │  Activity over time (1h)     │  │
│                     │  │  ▁▂▃▅▆▇█▇▆▅▄▃▂▁▂▃▄▅▆▇▆▅▄▃   │  │
│                     │  └──────────────────────────────┘  │
└─────────────────────┴────────────────────────────────────┘
```

---

## 🧠 Learning Through AI — The Methodology

This project was built using a deliberate **AI-assisted learning methodology** — a workflow designed to maximize *understanding*, not just output.

### The Core Philosophy

> The goal was never to get Claude to write the code. The goal was to use Claude to understand the code I was writing.

AI tools like Claude are powerful — and that power can easily become a crutch. The methodology used here inverts that trap by treating every AI interaction as a **teaching moment**, not a delegation.

---

### 🔄 The Workflow

#### 1. Struggle First
Before asking Claude anything, I attempted every feature or concept on my own. This wasn't about pride — it was about generating *genuine confusion*, which is the prerequisite for genuine learning. You can't ask good questions without first getting stuck.

#### 2. Explain the Stuck Point Precisely
When I hit a wall, I wrote out exactly what I was trying to do, what I expected, what I got instead, and what I'd already tried. This forced me to articulate my mental model — and often, that process alone revealed the bug.

#### 3. Ask for Explanation, Not Just Code
When I did ask Claude for help, I asked for *explanations* first. "Why does Rust require `Arc<Mutex<T>>` here instead of just `Mutex<T>`?" rather than "Fix my code." If Claude provided code, I required myself to explain it back in my own words before using it.

#### 4. Challenge and Probe
I treated Claude like a senior engineer who might be wrong. I pushed back, asked follow-up questions, requested alternative approaches, and asked "what are the tradeoffs of doing it this way?" This produced deeper understanding than any tutorial.

#### 5. Reproduce From Memory
After understanding a concept, I deleted Claude's explanation and re-implemented it from memory. If I couldn't, I went back and asked better questions.

#### 6. Document the Insight
Key learning moments were written down in plain English — not as code comments, but as conceptual notes that a future version of me (or a collaborator) could understand without running the code.

---

### 📚 Concepts Learned Through This Method

| Concept | How AI Helped |
|---|---|
| Rust ownership & borrowing | Socratic Q&A until the mental model clicked |
| Lifetimes in struct fields | Step-by-step lifetime elision rules explained conversationally |
| `Arc<Mutex<T>>` for shared state | Analogy-based explanation of interior mutability |
| `async`/`await` with `tokio` | Traced the event loop execution manually with Claude's guidance |
| `ratatui` widget system | Asked Claude to explain the Frame/Buffer model before writing a single widget |
| Terminal raw mode & ANSI codes | Explored what `crossterm` does under the hood |
| Trait objects vs generics | Asked for a concrete performance and ergonomic comparison |
| Error propagation with `?` | Traced through desugared `match` to understand what `?` compiles to |

---

### 💬 Example AI Interaction (Paraphrased)

**Me:** I'm getting a borrow checker error: "cannot borrow `app` as mutable because it is also borrowed as immutable." I'm inside a match arm that draws a widget, and I want to update state based on a keypress. What's happening?

**Claude:** The issue is that you're holding an immutable borrow on `app` (likely through a method that returns a reference to a field) at the same time as you're trying to call a `&mut self` method. Rust's borrow checker doesn't track borrows at the field level in all cases — it sees the whole struct as borrowed...

**Me:** Okay, but why can't it just see that the widget draw and the state mutation are in different arms of the match?

**Claude:** Great question. The borrow doesn't end at the arm boundary in this case because...

*[This back-and-forth continued for ~12 exchanges and produced a deep understanding of NLL (Non-Lexical Lifetimes) and how the borrow checker reasons about control flow.]*

---

## 🏗️ Architecture

```
src/
├── main.rs           # Entry point, terminal setup & teardown
├── app.rs            # Central App state struct
├── events/
│   ├── mod.rs        # Event loop (keyboard, tick, resize)
│   └── handler.rs    # Keybinding dispatch
├── ui/
│   ├── mod.rs        # Root render function
│   ├── layout.rs     # Panel constraints & layout helpers
│   └── widgets/      # Custom widget implementations
│       ├── chart.rs
│       ├── table.rs
│       └── gauge.rs
└── data/
    ├── mod.rs
    └── metrics.rs    # Data fetching & transformation
```

---

## 🚀 Getting Started

### Prerequisites

- Rust 1.75+ ([install via rustup](https://rustup.rs))
- A terminal emulator with Unicode and 256-color support

### Build & Run

```bash
git clone https://github.com/yourusername/rust-tui
cd rust-tui
cargo run --release
```

### Key Bindings

| Key | Action |
|---|---|
| `q` / `Ctrl+C` | Quit |
| `↑` / `k` | Navigate up |
| `↓` / `j` | Navigate down |
| `Tab` | Switch panel |
| `Enter` | Select / Confirm |
| `?` | Toggle help |

---

## 🔧 Dependencies

```toml
[dependencies]
ratatui    = "0.26"      # TUI rendering framework
crossterm  = "0.27"      # Cross-platform terminal control
tokio      = { version = "1", features = ["full"] }
anyhow     = "1"         # Ergonomic error handling
serde      = { version = "1", features = ["derive"] }
```

---

## 📖 What I'd Tell My Past Self

If I were starting this project again, I'd give myself three pieces of advice:

**1. Read the error messages properly.** Rust's compiler errors are documentation. The first week, I was pattern-matching on error shapes and asking Claude to fix them. The second week, I started reading them. By the third week, I could often see the fix before finishing the message.

**2. Understand the ownership model before writing any multi-threaded code.** I tried to add an async data pipeline before I understood why `Arc` existed. This created confusion that compounded. Learn single-threaded ownership first — deeply — then concurrency.

**3. Ask "why is this designed this way?" more than "how do I do this?"** The most valuable AI conversations weren't "how do I implement X" but "why did the Rust designers make this tradeoff?" Understanding intent makes the syntax memorable.

---

## 🤝 Contributing

Contributions, questions, and code reviews are welcome — especially from people also learning Rust. If you learned something from this codebase (or spotted something I misunderstood), open an issue.

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---

<p align="center">
  Built in Rust · Learned with AI · Understood by a human
</p>
