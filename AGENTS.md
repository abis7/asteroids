# AGENTS.md

## Overview
Single-file HTML5 Canvas clone of arcade Asteroids. No build system, no bundler, no dependencies, no tests, no lint — there is no `npm`/`package.json` here. All game logic lives in `game.js` (~420 lines), bootstrapped from `index.html` via `<script src="game.js">` with no modules.

## Run / verify
There is no test command. To verify changes, open `index.html` directly in a browser or run `npx serve .` — it serves on port 3000 unless that port is in use (check the CLI output for the actual URL/port). There is nothing to compile.

## Conventions & gotchas
- **Spanish**: all comments, UI copy (`SCORE`, `NIVEL`, overlays), and the README are in Spanish. Keep new text/strings in Spanish (`PUNTAJE`, `ESPACIO PARA REINICIAR`).
- **Canvas size is defined twice**: `index.html`'s `<canvas width height>` and the `W`/`H` constants in `game.js:5-6`. They must stay in sync (800×600).
- **Space is toroidal**: entity positions wrap around edges via the `wrap()` helper (`game.js:27`). Collision logic must keep using `wrap()`, not clamping.
- **Keypress edge detection**: `justPressed` + `pressed(code)` (`game.js:20-24`) fires once per press and clears itself. Use `pressed()` (not `keys[...]`) for discrete actions like shooting/restart; `keys[...]` only for held state (movement).
- **State machine**: global `state` is `'playing' | 'dead' | 'gameover'` (`game.js:241`); update branches per state. `dt` is clamped to 0.05s in the loop.
- **Asteroid sizes**: `RADII`/`SPEEDS`/`POINTS` arrays index by size 3 (large) → 2 → 1 (small) (`game.js:61-63`); `split()` halves size. `POINTS` matches the README scoring table.
- **Estrella fugaz**: asteroids can be spawned with `fugaz = true` (4th ctor arg) — ~3x faster, `ttl` 6 s (expires with a small explosion, `split()` returns `[]`), flat 150 pts instead of `POINTS[size]`, drawn as an orange comet with a tail. ~40% chance per `spawnAsteroids()`.
- **Power-ups**: only one power-up on screen at a time; spawned at 15% per asteroid destroyed. Two types, ~50/50: `'speed'` (orange `>>`, ship 2x thrust, cyan ship) and `'triple'` (magenta fan icon, ship fires 3 spread bullets ±0.15 rad, magenta ship). Both last 5 s via `ship.speedBoost`/`ship.tripleShot`; HUD bars via `drawBoostBar()` (`game.js`).
- Code style: `'use strict'`, ES6+ classes, `'use '` box-drawing section-header comments, globals for game state.