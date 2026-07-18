# Battleship vs AI

Playable URL: https://ishitajain15-source.github.io/battleship-vs-ai/

## How to play

Open `index.html` directly in a browser, or visit the playable URL above. Your fleet and the hidden enemy fleet are each placed randomly on 10×10 grids. You move first: click an untried enemy cell to fire. `MISS`, `HIT`, and `SUNK` results appear in the status line, while the fleet trackers show which ships remain afloat. The AI automatically takes its shot after a short delay. Sink all five enemy ships to win; use **New game** to start over. You may re-randomize your fleet only before firing your first shot.

## AI targeting

The AI begins in hunt mode, choosing a random cell it has not fired on. When it hits, it enters target mode and tries untried orthogonal neighbors. Once it has two hits on the same ship, it infers whether the ship is horizontal or vertical and works outward from both ends of that line. A sunk ship is removed from targeting; if other unresolved hits exist (including when ships touch), those are targeted before the AI returns to hunt mode. The AI tracks every fired coordinate, so it never repeats a shot.
