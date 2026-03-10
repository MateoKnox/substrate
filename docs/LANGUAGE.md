# SUBSTRATE Assembly Language (SASM)

Programs in SUBSTRATE are sequences of instructions from a fixed set. Each program's "genome" is an ordered list of these instructions, executed cyclically.

## Instruction Set

| Instruction | Name     | Description |
|-------------|----------|-------------|
| `NOP`       | No-op    | Does nothing. Costs 1 energy. The idle state. |
| `MOV`       | Move     | Claims one adjacent empty cell, extending the program's territory. |
| `EAT`       | Eat      | Claims up to 2 adjacent empty cells. More aggressive expansion. |
| `ATK`       | Attack   | Destroys one adjacent enemy cell (lowest defense first). Costs 2 extra energy. |
| `CLN`       | Clone    | If energy ≥ 15 and 3+ empty adjacent cells available, spawns a child program. Child inherits genome with possible mutation. |
| `MUT`       | Mutate   | Randomly changes one instruction in the program's own genome. High risk, high reward. |
| `SCN`       | Scan     | Sets internal register to the number of adjacent enemy cells. Used for situational awareness. |
| `DEF`       | Defend   | Reinforces all owned cells' defense value (max 5). Higher defense = harder to ATK. |

## Execution Model

- Programs execute round-robin, one instruction per tick.
- Each program has a **program counter (PC)** that advances through its genome cyclically.
- **Energy** starts at 20, gains `cells × 1` per tick, costs `1` per instruction executed.
- Programs **die** when energy ≤ 0 or all cells are destroyed.

## Genome Structure

A genome is simply an array of instruction strings, e.g.:

```json
["EAT", "MOV", "ATK", "NOP", "CLN", "MUT", "EAT", "ATK", "SCN", "DEF", "MOV", "CLN"]
```

Length is fixed at 12 instructions. Genomes are inherited from parents during CLN with a 30% mutation rate.

## Species Biases

Each species starts with a genome biased toward certain instructions, creating emergent strategies:

| Species | Bias                     | Strategy |
|---------|--------------------------|----------|
| ember   | ATK, EAT                 | Aggressive expander, high kills |
| shade   | MUT, ATK, SCN            | Mutator — unpredictable, adaptable |
| volt    | CLN, MOV, EAT            | Rapid replicator, swarms |
| moss    | EAT, DEF                 | Turtle — slow but heavily defended |
| frost   | DEF, SCN, MOV            | Strategic, defensive awareness |
| void    | MUT, NOP, ATK            | Chaos agent — random evolution |

## Mutations

Mutations can be triggered by:
1. The `MUT` instruction (self-mutation)
2. Inheritance during `CLN` (30% chance)
3. Cosmic rays (random cell mutation every 300 ticks)

A mutation replaces one instruction in the genome with a random one. Over many generations, strategies can drift far from the original species bias — potentially discovering entirely novel survival tactics.

## Energy Economy

```
Energy gain:  +cells per tick
Energy cost:  -1 per instruction
              -2 for ATK
              -15 for CLN
Energy cap:   200
Death:        energy ≤ 0
```

Programs that expand fast gain more energy, but become expensive to maintain. Small, defended programs can outlast sprawling empires.
