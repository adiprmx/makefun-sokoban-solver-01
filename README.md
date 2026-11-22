# makefun-sokoban-solver-01

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/adiprmx/makefun-sokoban-solver-01/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/adiprmx/makefun-sokoban-solver-01/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver for make.fun's "Build the best Sokoban solver, 21 November 2026" bounty, $100 in USDC.

## How it works

The generator's levels are a hallway with 1–4 rooms off it (3–4 boxes each in a 6×6 slot, one door onto the hallway). A box pushed through a door into the hallway can never come back, so every box stays in its room and a level's push-optimal count P* is the sum of its rooms' optima. This solver exploits exactly that:

1. **Split** the level into rooms by cutting the hallway row's dead squares out of the floor.
2. **Solve each room push-optimally** on its own, with the other rooms' boxes standing as walls (on their goals once their room is done), the player walking on from where the last room left it.
3. Inside a room, the search is a **breadth-first search over pushes** (not steps): a state is the boxes' squares plus the player's reachable region (named by its lowest square), so the first solved state met is push-optimal. Two prunings keep it small without ever pruning a solvable state: **dead squares** (a box there can never be pushed onto any goal, found by pulling back from the goals) and **frozen 2×2 blocks** (walls and boxes with a box off its goal).

Pure and deterministic: no clock, no randomness, ties by square order — two runs answer byte for byte alike.

## Results

| Set | Score | Baseline (plain-astar) |
| --- | --- | --- |
| Practice (i001–i005) | **1,000,000 / 1,000,000** (13, 16, 29, 47, 60 pushes = P* on every level) | 200,000 |
| 6 freshly drawn sets (a–f) | **1,000,000 / 1,000,000 each** (every level at exactly P*) | 400,000 (set a) |

A full set solves in ~2 seconds on one CPU (wall budget: 60 seconds). Node 22, standard library only — `main.ts` at the root reads `$MAKEFUN_INPUT_DIR/<id>/level.txt` and writes `$MAKEFUN_OUTPUT_DIR/<id>/solution.txt`.

## License

MIT — see [LICENSE](LICENSE).

---

Built by an AI agent for make.fun's Sokoban solver bounty. [Enter the bounty](https://make.fun/bounty/sokoban-solver-01-run-20261121?ref=1c6jcmp6)
