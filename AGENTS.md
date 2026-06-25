# Dansk

A static Danish language-learning website ("lær gennem leg" / learn through play). All content lives under `docs/` as self-contained HTML pages with inline CSS and vanilla JavaScript. There is **no backend, no build step, no package manager, and no dependencies**.

Pages:

- `docs/index.html` — public landing hub listing exercises and games.
- `docs/studio-k9m3.html` — internal catalog mirror (`noindex`), renders the same list via JS.
- `docs/exercises/puls-3-dialog-09-sheng-memorization.html` — Dialog 9 memorization exercise.
- `docs/exercises/praes-1-11-build-up-tts.html` — sentence build-up exercise with Danish TTS.
- `docs/games/taljagten-dansk-talspil.html` — Taljagten number-listening game.

Interactive audio uses the browser Web Speech API (`window.speechSynthesis`), not a server.

## Cursor Cloud specific instructions

- This project has no dependencies and nothing to install or build. The update script is intentionally a no-op.
- To run/develop, serve the `docs/` directory with any static HTTP server. The document root MUST be `docs/` (not the repo root) so relative links resolve:
  - `python3 -m http.server 8000 --directory docs`
- There are no lint, test, or build commands in this repo. "Testing" means opening pages in a browser and exercising the UI (e.g. play a Taljagten round, toggle translations on an exercise page).
- TTS audio will not produce sound in the headless cloud environment; UI interactions and visual state changes still work and are the correct way to verify functionality.
