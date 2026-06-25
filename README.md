# Dansk

A static Danish language-learning website ("lær gennem leg" / learn through play).

All content lives under `docs/` as self-contained HTML pages with inline CSS and vanilla JavaScript. There is no backend, no build step, and no dependencies.

## Pages

- `docs/index.html` — public landing hub listing exercises and games.
- `docs/studio-k9m3.html` — internal catalog mirror.
- `docs/exercises/puls-3-dialog-09-sheng-memorization.html` — Dialog 9 memorization exercise.
- `docs/exercises/praes-1-11-build-up-tts.html` — sentence build-up exercise with Danish TTS.
- `docs/games/taljagten-dansk-talspil.html` — Taljagten number-listening game.

## Running locally

Serve the `docs/` directory with any static HTTP server. The document root must be `docs/` so that relative links resolve correctly:

```
python3 -m http.server 8000 --directory docs
```

Then open `http://localhost:8000/`.

## Language policy

Do not use Portuguese anywhere in this project. Use Danish for the learning content and English for all user interface text, comments, and documentation.
