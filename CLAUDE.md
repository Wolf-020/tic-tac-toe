# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser game. No build step, no dependencies, no package manager. Open `tictactoe.html` directly in a browser to run it.

```bash
open tictactoe.html
```

## Git & GitHub workflow

Every meaningful change must be committed with a clean message and pushed to GitHub.

- Remote: `git@github.com:Wolf-020/tic-tac-toe.git` (SSH via `~/.ssh/github_key`)
- Branch: `main`
- Stage specific files — never `git add -A`
- Commit messages: imperative subject line (`Add X`, `Fix Y`, `Refactor Z`), body explaining why if non-obvious
- Always push after committing: `git push origin main`

## Code architecture

Everything lives in `tictactoe.html` in three inline sections:

**State (JS globals, line ~221)**
- `board` — flat 9-element array (`null | 'X' | 'O'`), indices 0–8 map left-to-right, top-to-bottom
- `current` — whose turn it is (`'X'` or `'O'`)
- `gameOver` — boolean gate; blocks clicks after a result is reached
- `scores` — `{ X, O, tie }` object persisted across games within a session

**Game logic**
- `WINS` — hardcoded array of all 8 winning index triplets (rows, cols, diagonals)
- `checkWinner()` — iterates `WINS`; returns `{ winner, line }` on a win, `{ winner: null, line: [] }` on a full board (tie), or `null` if the game is still in progress
- `handleClick()` — single entry point for all moves; calls `checkWinner()` after each placement and updates state accordingly
- `restart()` — resets `board`, `current`, `gameOver` but preserves `scores`

**Rendering**
- `render()` — fully redraws all 9 cells and the scoreboard from state; called after every state change
- CSS classes drive visual state: `.taken`, `.x`/`.o`, `.win-cell` are added/removed by `render()` and `handleClick()`
- Animations (`pop`, `pulse`) are CSS keyframes triggered by class application — no JS animation logic
