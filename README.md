```
 ███████╗██╗   ██╗██████╗ ███████╗████████╗██████╗  █████╗ ████████╗███████╗
 ██╔════╝██║   ██║██╔══██╗██╔════╝╚══██╔══╝██╔══██╗██╔══██╗╚══██╔══╝██╔════╝
 ███████╗██║   ██║██████╔╝███████╗   ██║   ██████╔╝███████║   ██║   █████╗
 ╚════██║██║   ██║██╔══██╗╚════██║   ██║   ██╔══██╗██╔══██║   ██║   ██╔══╝
 ███████║╚██████╔╝██████╔╝███████║   ██║   ██║  ██║██║  ██║   ██║   ███████╗
 ╚══════╝ ╚═════╝ ╚═════╝ ╚══════╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝   ╚═╝   ╚══════╝
```

> **autonomous programs fight for survival inside a simulated memory grid**

Six species of programs hatch into a shared memory space. They expand, attack, replicate, mutate. The fittest code survives. The weak gets overwritten. No rules, no referee — just raw competition and genetic drift.

---

## What is this

SUBSTRATE is a zero-dependency Node.js simulation. Programs are sequences of instructions from a custom assembly language (SASM). They execute round-robin, one instruction per tick. They can claim empty cells, attack enemies, clone themselves into adjacent space, and mutate their own genome.

Watch a turtle-strategy `moss` program build a fortress while a frenzied `ember` swarm burns through the grid — only to be outlasted by a third-generation `shade` mutant that stumbled onto a perfect survival sequence.

```
tick 1847 | programs: 23 | kills: 412 | extinct: 1

╔══════════════════════════════════════════════════╗
│ ░░░░░░░░░░░░░···········▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ember:1   L0 cells:148
│ ░░░░░░░░░░░░░░░·········▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ██████░░░░ 82 energy
│ ░░░░░░░░░░░░░░░░░·······▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ──────────────────────
│ ░░░░░░░░░░░░░░░░░░░·····▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  shade:3   L2 cells:91
│ ░░░░░░░░░░░░░░░░░░·▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ████░░░░░░ 61 energy
│ ░░░░░░░░░░░░░░░░░░·▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ──────────────────────
│ ░░░░░░░░░░░░░░░░░·▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  moss:1    L0 cells:77
│ ░░░░░░░░░░░░░░░░·▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │  ███░░░░░░░ 44 energy
│ ···░░░░░░░░░░░·▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒ │
│ ·····░░░░░░░·▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒ │  EVENTS
│ ·▪▪▪▪▪·····▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒ │  ⚔ ember:1 killed shade
│ ·▪▪▪▪▪▪▪▪·▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒ │  🧬 volt:2 spawned volt:9
│ ·▪▪▪▪▪▪▪▪▪▪▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒▒ │  ☢ shade:3 mutated
│ ·▪▪▪▪▪▪▪▪▪▪▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▒▒▒▒ │  💀 void:1 was eliminated
╚══════════════════════════════════════════════════╝
```

---

## Quick start

```bash
# Clone
git clone https://github.com/MateoKnox/substrate
cd substrate

# Start the simulation engine (terminal 1)
node engine/world.js

# Watch in the terminal (terminal 2)
node cli.js

# OR open the web view (terminal 2)
node serve.js
# → open http://localhost:3000
```

No `npm install` needed. Zero dependencies.

---

## The six species

| Species | Character | Color  | Strategy |
|---------|-----------|--------|----------|
| ember   | `█`       | red    | Aggressor — high ATK bias, burns through enemies |
| shade   | `▓`       | purple | Mutator — MUT-heavy, evolves unpredictably |
| volt    | `▒`       | yellow | Replicator — CLN-heavy, floods the grid with offspring |
| moss    | `░`       | green  | Fortress — EAT+DEF, slow expansion, hard to kill |
| frost   | `▪`       | blue   | Tactician — DEF+SCN, reads the battlefield |
| void    | `▫`       | grey   | Chaos — random mutations, unpredictable outcomes |

---

## How it works

**Memory** is an 80×40 grid of cells. Each cell is either empty or owned by a program.

**Programs** are sequences of 8 possible instructions (SASM):

```
NOP  → do nothing
MOV  → claim 1 adjacent empty cell
EAT  → claim up to 2 adjacent empty cells
ATK  → destroy 1 adjacent enemy cell
CLN  → spawn a child program (costs 15 energy)
MUT  → randomly change one instruction in own genome
SCN  → scan surroundings, count enemy neighbors
DEF  → reinforce all own cells against attack
```

**Energy** — programs gain 1 energy per owned cell per tick, spend 1 per instruction. Run out of energy: die. Lose all cells: die.

**Evolution** — when CLN fires, the child inherits the parent's genome with a 30% mutation chance. Mutations accumulate across generations. A program 5 generations deep might behave nothing like its ancestor.

**Cosmic rays** — every 300 ticks, a random cell in the grid gets its instruction mutated. Occasionally this rescues a dying program, or dooms a dominant one.

---

## Architecture

```
engine/
├── memory.js      — grid, cell management
├── programs.js    — species, genomes, lifecycle
├── vm.js          — SASM instruction execution
└── world.js       — main loop, state saving

cli.js             — ANSI terminal renderer
index.html         — web frontend (canvas, zero deps)
serve.js           — local dev HTTP server
world-state.json   — live state file (engine → frontends)

docs/
├── LANGUAGE.md    — SASM instruction set reference
└── ARCHITECTURE.md — technical deep-dive
```

---

## Origin

This entire codebase — the VM, evolution engine, terminal renderer, and web frontend — was designed and written autonomously by an AI agent running on [OpenClaw](https://openclaw.ai). Every file, every design decision, every instruction in the SASM spec.

---

## License

MIT
