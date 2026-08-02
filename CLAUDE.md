# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, dependency-free mini game hub in Korean. `index.html` is a landing page linking to five self-contained HTML games under `games/`. There is no build step, package manager, or test suite — every page is plain HTML/CSS/JS opened directly in a browser.

## Development

- No install/build/lint/test commands exist. Open `index.html` (or any file in `games/`) directly in a browser to see changes.
- To preview via a local server instead of `file://`: `python -m http.server` from the repo root, then visit `http://localhost:8000`.

## Architecture

- **`index.html`** — landing page with a card grid; each card links to `games/<name>.html`.
- **`games/*.html`** — one game per file, fully self-contained (inline `<style>` and `<script>`, no external JS/CSS files, no shared modules). Game logic is wrapped in an IIFE and driven by `canvas` (snake) or DOM elements (2048, minesweeper, memory, tictactoe).
- Every game page has a `← 홈으로` link back to `../index.html`, and the whole UI (including page text) is in Korean — keep new pages consistent with that.
- All pages share the same CSS custom-property theme (dark palette: `--bg`, `--panel`, `--cell`, `--accent`, `--danger`, `--success`, etc.) defined redundantly in each file's `:root`. There is no shared stylesheet, so if the palette changes it must be updated in every HTML file individually.
- Adding a new game means: create `games/<name>.html` following the existing self-contained pattern, then add a matching `.card` entry to the grid in `index.html`.

## Auto-commit hook

`.claude/settings.json` defines a `PostToolUse` hook (on `Write|Edit|NotebookEdit`) that automatically runs `git add -A`, commits as `Auto-commit: <timestamp>`, and pushes to `origin main` after every file edit. This means edits in this repo are committed and pushed to GitHub immediately and automatically — there is no staging/review step before push.
