# SUBSTRATE Architecture

## Overview

SUBSTRATE is a living simulation where autonomous programs compete for survival inside a virtual memory grid. The architecture is split into three layers: the simulation engine, the terminal UI, and the web frontend.

```
engine/world.js  ──→  world-state.json  ──→  index.html (canvas)
      │                                              │
      └──────────────────────────────────────────── cli.js (ANSI terminal)
```

The engine is the single source of truth. It writes `world-state.json` every 25 ticks. Both the terminal UI and web frontend read this file independently — no sockets, no framework, no shared state.

## Components

### engine/memory.js
The memory grid — an 80×40 2D array of cells. Each cell tracks:
- `owner`: which program ID owns this cell (or null if empty)
- `instruction`: the instruction stored in this cell
- `defense`: reinforcement level (0–5), raised by DEF instruction

Provides helpers for adjacent cell lookup, enemy detection, and empty space finding.

### engine/programs.js
Program definitions, species data, genome generation, and cloning logic.

Species define a bias toward certain instructions — this creates starting strategy diversity without hardcoding behavior. Genomes are arrays of SASM instructions.

### engine/vm.js
The virtual machine that executes SASM instructions. Each instruction is handled as a pure function operating on the program, memory, and program list. Returns new programs (from CLN) and changed cells.

Maintains an in-memory event log of significant simulation moments.

### engine/world.js
The main simulation loop:
1. **Energy gain** — each alive program gains energy proportional to cells owned
2. **Execution** — programs execute in shuffled order (fairness)
3. **Reaping** — dead programs (energy ≤ 0 or no cells) are cleared
4. **Cosmic rays** — random mutation injection every 300 ticks
5. **State save** — world-state.json written every 25 ticks

### cli.js
Full ANSI terminal renderer. Reads world-state.json, renders:
- The 80×40 memory grid with species-colored characters
- Species population bars
- Top programs leaderboard
- Live event log

Runs at ~150ms refresh rate. Uses 24-bit ANSI color codes.

### index.html
Single-file web frontend. Canvas-based renderer with:
- Smooth polling of world-state.json (400ms)
- Cell change detection → pulse animation on active frontlines
- Hover tooltip showing program details
- Species bars, top programs list, event log
- Fully offline-capable (shows helpful message when engine isn't running)

### serve.js
Minimal Node.js HTTP server (zero dependencies). Serves index.html and world-state.json locally.

## Data Flow

```
[Tick loop 80ms]
  ├─ gain energy (all programs)
  ├─ shuffle program order
  ├─ execute one instruction per program
  │    └─ may produce: new child programs, cell changes, events
  ├─ reap dead programs
  ├─ cosmic ray (every 300 ticks)
  └─ save world-state.json (every 25 ticks)

[CLI 150ms]
  └─ read world-state.json → render ANSI grid

[Web 400ms]
  └─ fetch world-state.json → update canvas + sidebar
```

## Emergent Behaviors

The simulation has no scripted story. What happens emerges from:

- **Strategy conflicts**: aggressive ATK-heavy programs vs. defensive DEF+EAT programs
- **Genetic drift**: MUT accumulates changes; programs diverge from species origin
- **Energy economics**: sprawling empires collapse when expansion outpaces income
- **Positional advantage**: corner programs have fewer attack surfaces
- **Cosmic mutation**: random gene edits can accidentally create superior strategies

The richest simulations tend to develop a late-game phase where a heavily-mutated third-generation program outlasts all pure-strategy first-generation programs.
