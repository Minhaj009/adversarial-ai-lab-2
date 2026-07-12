# Adversarial AI Lab — Minimax & Alpha-Beta with Pac-Man vs Ghost

A single-file, interactive web app that teaches **adversarial search** (Minimax and Alpha-Beta pruning) through a playable 5×5 grid game of **Pac-Man vs Ghost**.

## Overview

Two agents move on a 5×5 grid:

- **Ghost** — always a **greedy chaser**: it takes the single move that minimizes its Manhattan distance to Pac-Man. It does *not* search.
- **Pac-Man** — strategy is user-selectable:
  - **Greedy**: rushes the nearest food dot and ignores the ghost. Usually gets caught.
  - **Safe (minimax)**: re-runs a depth-5 minimax that *honestly models the ghost as the same greedy chaser*. Because both agents are optimal, the result is a guaranteed **draw**, never a win.

This makes the two search methods directly comparable: greedy play is fast but risky, while a safe minimax can only guarantee a draw against an equally optimal opponent.

## Rules

- Pac-Man wins by eating **all food** (2–3 dots placed randomly, never on the starting corners).
- Ghost wins by landing on the same square as Pac-Man.
- If a position repeats (the agents cycle with no progress) or the turn cap (80) is reached, the result is a genuine **draw** — never an automatic win.

## How to run

The app is one self-contained HTML file (`adversarial-ai-lab.html`) — no build step, no dependencies to install. It inlines React 18, ReactDOM, a Babel classic runtime + `_jsx`/`_jsxs` shim, and the Tailwind styles.

To run it locally, clone the repo and serve it over HTTP (a local server is recommended over `file://`):

```powershell
git clone https://github.com/Minhaj009/adversarial-ai-lab-2.git
cd adversarial-ai-lab-2
python -m http.server 8000
```

Then open:

```
http://localhost:8000/adversarial-ai-lab.html
```

## Using the lab

1. Use the **Pac-Man strategy** toggle (Greedy / Safe) above the grid.
2. Press **Calculate Next Move** to pause and project the game forward. The "ghostly overlay" shows the predicted line of play 2–3 moves ahead; the real move animates on the board.
3. Watch the sidebar for the **α/β bounds**, nodes evaluated, and pruning count to see Alpha-Beta in action.
4. Switch strategies and re-run to compare outcomes:
   - **Greedy** Pac-Man → the chaser usually catches it.
   - **Safe** Pac-Man → a real draw.

## How the search works

- **SAFE mode** runs minimax: Pac-Man explores its own moves while the Ghost is modeled as a greedy chaser that always steps to the square closest to Pac-Man.
- **GREEDY mode** simply moves Pac-Man toward the nearest dot, ignoring the ghost.

### Evaluation function

- **Safe**: `score = (food eaten × 10) − (distance to ghost × 20)`. A capture collapses the score to `−1000`.
- **Greedy**: `score = −(distance to nearest dot)`.

## Files

| File | Purpose |
|------|---------|
| `adversarial-ai-lab.html` | The complete, self-contained lab (logic + UI + styles). |
| `README.md` | This file. |

## Notes for instructors

The "Safe Pac-Man always draws" outcome is a **validated property**, not a bug: against an optimal greedy chaser, an optimal minimax Pac-Man can guarantee at most a draw. This is a clean, intuitive demonstration that adversarial search's value depends on the *opponent model* — and a useful lead-in to discussing why stronger opponents or deeper search change the picture.
