# Six Men's Morris AI Agent

An AI agent that plays **Six Men's Morris** (a smaller variant of Nine
Men's Morris, played on two concentric squares with 16 points and 6
pieces per player), built for **CS814 — AI for Autonomous Systems**.

Per the task specification, the **"flying" rule is not used** — in the
movement phase a piece may only slide to an *adjacent* empty point.

The agent implements:
- A full game engine: placement/movement phases, mill detection, and the
  standard capture rule.
- A heuristic **evaluation function** (piece advantage, mills, potential
  mills, mobility).
- **Minimax search with alpha-beta pruning** (configurable depth, with
  mill-forming move ordering for better pruning).
- **Monte Carlo Tree Search (MCTS)** with UCT selection.
- Human-vs-AI and AI-vs-AI (alpha-beta vs. MCTS) game loops with a
  text-based board display.

The notebook is organised task by task, with a short explanation of the
design decisions before each implementation.

## Bugs fixed from the original version

- **`MILLS` included 6 invalid, non-collinear patterns** (e.g.
  `[1, 4, 11]`, where 4 and 11 aren't even adjacent on the board),
  causing false mill detections. Corrected to the 8 real mills — the
  four edges of each of the two concentric squares.
- **`is_terminal()` / `get_winner()` called `self.get_valid_actions()`
  as a method**, but it's a module-level function — this raised an
  `AttributeError` the moment any game reached the movement phase,
  which stopped both Human-vs-AI and MCTS from completing.
- **MCTS backpropagation credited every node's wins to a single fixed
  player** instead of the player who actually moved into that node,
  which broke UCT's minimax alternation.

All three were verified fixed by running full AI-vs-AI games end to
end (confirming the movement phase no longer crashes and games reach
a real decisive winner).

## Enhancements

- **Move ordering** in alpha-beta (mill-forming actions tried first) for
  earlier cutoffs and faster search.
- A move-count safety cap on human-vs-AI games, matching the one
  AI-vs-AI already had.
- Per-task explanations and fuller code comments throughout.

## Running it

No external dependencies — pure Python standard library (`math`,
`random`, `typing`). Open `six_mens_morris_ai.ipynb` in Jupyter and run
the cells top to bottom, then run the final cell to play. The menu
offers:

1. Human vs AI (choose your side and the AI's search depth)
2. AI vs AI (Alpha-Beta vs MCTS)
3. A single-position evaluation-function test
