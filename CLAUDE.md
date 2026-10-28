# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vanilla JS Tetris (HTML5 Canvas). No build, no deps, no package.json, no tests, no linter. README is in Spanish; UI strings are Spanish too.

## Run

Open `index.html` directly, or serve statically: `python -m http.server 8000` → http://localhost:8000.

## Architecture

All logic is in `game.js` (single script, global state, `'use strict'`); `index.html` supplies DOM ids it looks up at load (`board`, `next-canvas`, `score`, `lines`, `level`, `overlay`, `overlay-title`, `overlay-score`, `restart-btn`, `theme-toggle`, `name-entry`, `name-input`, `records`, `records-list`, `records-stats`, `reset-records-btn`).

- Board: `ROWS×COLS` matrix; `0` = empty, `1–7` = index into `COLORS`/`PIECES` (index 0 is `null` in both).
- Piece: `{type, shape, x, y}`; `shape` is a matrix holding its own color index. Rotation via `rotateCW` + simple kicks `[0,-1,1,-2,2]` in `tryRotate`.
- Flow: `init()` → rAF `loop` accumulates dt vs `dropInterval` → gravity or `lockPiece()` (`merge` → `clearLines` → `spawn`). `spawn()` calling `endGame()` on collision is the game-over condition.
- Pause/game over both use the same `#overlay`; `togglePause` and `endGame` cancel/restart the rAF (`animId`). `init` is also the restart handler.
- Speed: `dropInterval = max(100, 1000 - (level-1)*90)`; level = `floor(lines/10)+1`.

## Gotchas

- Changing `COLS`/`ROWS`/`BLOCK` requires manually updating the `<canvas id="board">` width/height in `index.html` (`COLS*BLOCK` × `ROWS*BLOCK`). The next-piece canvas is a fixed 120×120 (4×4 cells of 30px, hardcoded `NB` in `drawNext`).
- Adding/removing a piece type means updating `COLORS`, `PIECES`, and the `* 7 + 1` in `randomPiece`.

## Records

Sección `// ---- Records ----` en `game.js`. Start screen = `#overlay` mostrado al cargar (`showStartScreen`, no se llama `init()` hasta pulsar Jugar; `gameOver=true` bloquea teclas). `localStorage['tetris-records']` = `{top:[{name,score,lines,combo,date}], bestCombo, maxLines}`. Combo se cuenta en `clearLines` vía `trackCombo`. Keydown ignora eventos con target `#name-input`.
