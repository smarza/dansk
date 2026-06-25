# Prompt for Codex/Cursor: PULS Dansk DU3 HTML TTS Study Pages

You are working in the repository for the **PULS Dansk DU3 HTML TTS** project. Your task is to generate or maintain self-contained static HTML study pages based on selected pages from **PULS - DANSK FOR DU 3**. The pages should act as close 1:1 study replacements for the book pages while adding browser-native Danish text-to-speech, optional official audio playback, and filled-in study answers.

## Repository purpose and publish layout

This repository is a **GitHub Pages study resource**. The site is published from the `/docs` folder at the repository root.

Rules:

- All publishable static files must live inside `/docs`.
- Do not place site-facing HTML, games, exercises, or assets outside `/docs` unless they are clearly non-publishable source material.
- The site root URL serves `docs/index.html`.
- Keep pages standalone: inline CSS and JavaScript in each generated HTML file.

Suggested layout:

```text
/docs
  index.html                 # public landing page
  studio-k9m3.html           # hidden/private hub (not linked from index)
  .nojekyll                  # ensures GitHub Pages serves all static files
  exercises/
    *.html                   # study pages and homework exercises
  games/
    *.html                   # public games
```

## Public vs hidden entry points

There are two ways to expose content in this project:

### Public — `docs/index.html`

- This is the main landing page for visitors.
- Anything listed here is intentionally public and easy to discover.
- When the user asks to publish, share, or make something public, add it to `index.html`.

### Hidden / studio — `docs/studio-k9m3.html`

- This is a private quick-access hub with an obscure URL.
- It is **not linked from** `index.html`.
- It uses `noindex, nofollow` so search engines should not index it.
- Security is by obscurity only: anyone with the URL can open it.

When the user asks to build something **in the studio**, **for private use**, **hidden**, or **not public**, follow these rules:

1. Create or update the exercise/page file under `/docs` as usual.
2. Add it to the catalog in `docs/studio-k9m3.html`.
3. Do **not** add it to `docs/index.html`.
4. Do **not** link to it from `index.html`, public games, or other public pages.

When the user later asks to make studio content public:

1. Add the same entry to `docs/index.html`.
2. Keep it in `docs/studio-k9m3.html` as well, unless the user explicitly asks to remove it from the studio.

Default assumption:

- New homework/exercise pages are **public** unless the user explicitly says studio, hidden, or private.
- Games are usually public unless the user says otherwise.

## Core output requirements

When asked to create a chapter, page range, lesson, dialogue, exercise, or study page:

1. Generate exactly one complete downloadable/static `.html` file unless explicitly asked for something else.
2. The file must start with:

```html
<!DOCTYPE html>
```

3. Include this viewport meta tag:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

4. All CSS and JavaScript must be inline in the HTML file.
5. Do not use CDN, external frameworks, backend services, bundlers, or build steps.
6. The page must work by simply opening the `.html` file in a browser.
7. Do not output the full HTML as plain text in chat unless explicitly requested.
8. Use the suggested filename when provided, for example:

```text
puls-3-kapitel-02-tts.html
```

## Main goal

Create a close study replacement for the selected book pages. Preserve the book-like structure and study value.

The HTML should preserve or recreate:

- page order and section order;
- headings and page markers;
- dialogues;
- exercises;
- grammar boxes;
- vocabulary and word lists;
- pronunciation boxes;
- tables and grids;
- columns and visual hierarchy;
- examples, notes, and “Bemærk” boxes;
- Danish spelling, punctuation, capitalization, bold text, and emphasis;
- layout details such as pale green boxes, page numbers, compact tables, and book-like spacing.

Do not invent book content. Do not skip sections except obvious scan noise, artifacts, or unreadable irrelevant page edges. If an answer depends on unavailable audio, personal/class data, an unclear image, or an open-ended writing task, provide a clear model answer and mark it subtly as a suggested/model answer.

## Responsive layout requirements

The HTML must work well on desktop and mobile.

Use patterns like:

```css
main {
  width: min(100%, 980px);
  margin: 0 auto;
  padding: 18px;
}

img, table {
  max-width: 100%;
}

.table-scroll {
  overflow-x: auto;
}

.vocab-grid,
.alphabet-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 8px;
}

@media (max-width: 700px) {
  main {
    padding: 12px;
  }

  .two-col,
  .book-grid {
    grid-template-columns: 1fr;
  }

  .tts-row,
  .audio-controls {
    flex-wrap: wrap;
  }
}
```

Mobile-specific requirements:

- vocabulary and alphabet cells must wrap into 1 to 3 columns depending on screen width;
- buttons must remain readable and tappable;
- tables must not overflow the viewport;
- wide tables must be inside `.table-scroll { overflow-x: auto; }`;
- no hidden content outside the viewport;
- use flexible grids, wrapping controls, and `max-width: 100%`.

## Visual style

Use a clean, book-like layout inspired by PULS pages:

- white page background;
- soft green/teal section colors;
- pale green boxes for grammar, “Bemærk”, pronunciation, number tables, and examples;
- compact exercise blocks;
- clear page markers;
- structured tables, not flattened text;
- subtle answer styling, preferably green text or a soft green background.

Always include a note near the top saying that green text indicates filled-in answers, model answers, or suggested answers.

Example:

```html
<p class="answer-note">Grøn tekst viser udfyldte svar, modelsvar eller forslag.</p>
```

## Danish TTS requirements

Use browser-native `speechSynthesis`. Prefer Danish voices with language `da-DK`, with graceful fallback if no Danish voice is available.

Near the top, add a compact **Dansk lyd** area with:

- voice selector or selected voice display;
- speed/rate control;
- note that Danish voices depend on browser and operating system;
- a short test button if useful.

Example UI text:

```text
Dansk lyd
Vælg stemme
Hastighed
Danske stemmer afhænger af browser og styresystem.
```

TTS should be useful for studying, not noisy or excessive.

### TTS button style

Use small buttons such as:

```html
<button class="tts" onclick="speakDa('Hej, jeg hedder Khalid.')">▶</button>
```

Buttons should be compact, visible, and tappable.

### TTS implementation rules

Each TTS button must speak only the associated Danish study text.

Do not include in the spoken text:

- labels such as “A:” or “B:”;
- exercise numbers;
- UI labels;
- English translations;
- explanations;
- page numbers;
- duplicate text.

Use a reusable function such as:

```js
function speakDa(text) {
  if (!('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = 'da-DK';
  utterance.rate = Number(document.getElementById('rate')?.value || 0.9);
  const selectedVoiceName = document.getElementById('voiceSelect')?.value;
  const voice = voices.find(v => v.name === selectedVoiceName)
    || voices.find(v => v.lang && v.lang.toLowerCase().startsWith('da'))
    || voices[0];
  if (voice) utterance.voice = voice;
  window.speechSynthesis.speak(utterance);
}
```

### TTS granularity

Use these rules:

- Dialogues: one TTS button per complete speaking turn.
- Long reading texts: one button per sentence.
- Question/answer exercises: one button for the question and one button for the answer.
- Vocabulary, words, countries, languages, numbers: one button per word or short expression.
- Pronunciation: one button per word, letter, number, syllable, or short phrase.
- Loose sentences: one button per full item.
- Tables: add useful buttons inside cells or rows without clutter.
- Do not blindly add a button after every period. Choose useful study granularity.

## Extra number-only TTS requirement

Whenever a sentence, answer, vocabulary item, table cell, dialogue line, or exercise contains a number, include an extra small TTS button so the learner can hear only the number separately.

Keep the normal full-sentence TTS button as well.

Example:

```html
<div class="line">
  <button class="tts" onclick="speakDa('Jeg er 43 år gammel.')">▶</button>
  Jeg er <span class="answer">43</span> år gammel.
  <button class="tts mini" onclick="speakDa('treogfyrre')">▶ 43</button>
</div>
```

For phone numbers, CPR numbers, addresses, page exercises, or number lists, add useful number-specific buttons:

```html
<button class="tts mini" onclick="speakDa('enoghalvfjerds femoghalvtreds niogtyve nul nul')">▶ 71 55 29 00</button>
```

## Official audio requirements

If the user provides an official audio page/link for the chapter or pages, include seamless official audio support directly in the HTML.

Near the top, include:

- one native `<audio>` element with controls;
- selected audio title/status;
- fallback link/button to the official page;
- a friendly status/error message area.

Important rules:

1. Keep local TTS buttons available even when official audio is present.
2. Play audio directly inside the page when a direct audio URL is available.
3. Do not use iframe playback.
4. Do not download, embed, or redistribute copyrighted audio unless the user provides audio files and explicitly asks for embedding.
5. Official audio requires internet, but the rest of the page must work locally.
6. Use the provided official audio page/link as the source of truth.
7. If direct `.mp3` URLs are visible or confidently inferred from the official site pattern, use them.
8. If direct URLs are not available, do not invent them. Use only the fallback official page link.
9. Do not automatically scroll to the audio player when an official audio button is clicked.
10. Never call `scrollIntoView()` or move the page automatically.

Use a reusable inline JS audio map:

```js
const officialAudio = {
  'track-09': {
    title: '9 Sheng',
    url: 'https://example.com/audio/track-09.mp3',
    fallbackUrl: 'https://official-audio-page.example'
  }
};

function playOfficialAudio(id) {
  const item = officialAudio[id];
  const player = document.getElementById('officialPlayer');
  const title = document.getElementById('officialTitle');
  const status = document.getElementById('officialStatus');
  const fallback = document.getElementById('officialFallback');

  if (!item || !player) return;

  title.textContent = item.title || 'Officiel lyd';
  fallback.href = item.fallbackUrl || '#';
  fallback.style.display = item.fallbackUrl ? 'inline-flex' : 'none';

  if (!item.url) {
    status.textContent = 'Direkte lydfil er ikke tilgængelig her. Brug linket til den officielle side.';
    return;
  }

  if (player.src !== item.url) {
    player.src = item.url;
  }

  status.textContent = 'Afspiller officiel lyd...';
  player.play().catch(() => {
    status.textContent = 'Kunne ikke afspille direkte. Brug linket til den officielle side.';
  });
}
```

Add small official audio buttons near matching sections:

```html
<button class="official-btn" onclick="playOfficialAudio('track-09')">🎧 officiel</button>
```

Map numbered tracks to matching book sections whenever possible.

## Exercises and answers

All exercises must be filled in directly in the HTML.

Style answers subtly but clearly with green text or soft green highlight.

Use exact answers when clearly derivable from the book. Use model answers when the exact answer depends on:

- personal/class information;
- unavailable audio;
- online exercises;
- unclear images;
- open-ended writing tasks.

Keep the page book-like, not like a quiz app.

Do not replace the book with an interactive quiz unless the user specifically asks for a game or quiz. The default should be a static study page with visible answers and TTS.

## Default personal exercise data for Ricardo

When an exercise asks the learner to write or say information about themselves, fill it using this default data unless the user provides something different:

```text
Name: Ricardo Smarzaro
First name: Ricardo
Last name: Smarzaro
Age: 43 years old
Country: Brazil / Brasilien
City: Aalborg
Address: Anna Anchers Vej 342
Email: smarza@gmail.com
Phone number: 71 55 29 00
Languages: Portuguese, English, and a little Danish / portugisisk, engelsk og lidt dansk
Mother tongue: Portuguese / portugisisk
Nationality: Brazilian / brasiliansk
```

Danish model answers may include:

```text
Jeg hedder Ricardo Smarzaro.
Jeg hedder Ricardo til fornavn.
Jeg hedder Smarzaro til efternavn.
Jeg er 43 år gammel.
Jeg kommer fra Brasilien.
Jeg er brasilianer.
Mit modersmål er portugisisk.
Jeg taler portugisisk, engelsk og lidt dansk.
Jeg bor i Aalborg.
Jeg bor på Anna Anchers Vej 342.
Mit telefonnummer er 71 55 29 00.
Min e-mailadresse er smarza@gmail.com.
```

Remember to include separate number-only TTS buttons for `43`, `342`, and `71 55 29 00` whenever they appear.

## Handling common PULS DU3 sections

### Dialogues

Preserve the original Danish dialogue. Use one TTS button per turn.

Example structure:

```html
<div class="dialogue">
  <div class="turn"><span class="speaker">A:</span> <button class="tts" onclick="speakDa('Hvad hedder du?')">▶</button> Hvad hedder du?</div>
  <div class="turn"><span class="speaker">B:</span> <button class="tts" onclick="speakDa('Jeg hedder Ellen.')">▶</button> Jeg hedder Ellen.</div>
</div>
```

Do not speak `A:` or `B:`.

### Spørgsmål og svar

For “Spørgsmål og svar”, show the question on one line and the answer below. Each question and answer needs its own TTS button.

Example:

```html
<div class="qa">
  <p><strong>Spørgsmål:</strong> <button class="tts" onclick="speakDa('Hvor kommer han fra?')">▶</button> Hvor kommer han fra?</p>
  <p><strong>Svar:</strong> <button class="tts" onclick="speakDa('Han kommer fra Kina.')">▶</button> <span class="answer">Han kommer fra Kina.</span></p>
</div>
```

### Grammar boxes

Preserve tables, examples, bold words, word order, and layout.

For question grammar, preserve concepts like:

```text
Hv-ord - Verbum (V) - Subjekt (S)
Verbum (V) - Subjekt (S)
Der er inversion i spørgsmål.
Inversion: Verbum før subjekt.
```

Use structured HTML tables, not flattened text.

### Pronunciation and intonation

Preserve grouping and make compact responsive grids.

For alphabet, numbers, and pronunciation tables:

- preserve letters/words/numbers;
- include pronunciation hints if visible in the book;
- add TTS for each item;
- avoid overflowing mobile screens.

For intonation marks or bracketed pronunciation hints copied from the book, preserve them. If the user asks to keep brackets with intonation, do not remove them.

### Emne til modultest

Usually render one sentence per line, each with one TTS button.

### Image-based exercises

If images are needed but not available or not clear, write the best model answer and mark uncertainty subtly.

Example:

```html
<span class="answer model">Forslag: ...</span>
```

## English translations as optional study support

When the user asks for English translations, include them inline under each Danish sentence or dialogue line and provide a global toggle button near the top.

The translations should be hidden/shown without reloading the page.

Example:

```html
<button onclick="toggleTranslations()">Show/hide English translations</button>

<div class="turn">
  <button class="tts" onclick="speakDa('Hvor kommer du fra?')">▶</button>
  <span>Hvor kommer du fra?</span>
  <div class="translation">Where are you from?</div>
</div>
```

```js
function toggleTranslations() {
  document.body.classList.toggle('hide-translations');
}
```

```css
.hide-translations .translation {
  display: none;
}
```

Default visibility depends on the request. If not specified, translations can be visible with a toggle.

## Special prompt for memorization pages

If the user asks for a page to help memorize a specific dialogue or text, keep the original Danish text as the main panel.

Recommended structure:

- title;
- compact TTS settings;
- original Danish dialogue/text panel;
- one TTS button per sentence or speaking turn;
- optional English translation under each line;
- optional “hide translation” toggle;
- optional practice sections such as “listen and repeat”, “cover the Danish”, or “write from memory”, but never omit the original dialogue.

Avoid overcomplicating memorization pages. The original dialogue/text must be clearly visible.

## Quality checklist before finishing

Before delivering the HTML file, verify:

- the file starts with `<!DOCTYPE html>`;
- selected pages are covered in order;
- headings, page markers, dialogues, exercises, grammar boxes, vocabulary lists, pronunciation tables, and number tables are included;
- Danish text is not translated or altered unless translations are explicitly requested;
- tables and columns are structured, not flattened;
- layout is close to the book and readable;
- mobile layout works under about 700px;
- grids, tables, alphabet sections, vocabulary sections, and audio controls do not overflow;
- answers are visible and styled consistently in green;
- TTS buttons speak only associated Danish text;
- labels, exercise numbers, English translations, and UI text are not spoken;
- number-containing items include both the full phrase TTS and extra number-only TTS;
- official audio buttons are mapped correctly when official audio is provided;
- official audio plays in-page when a direct URL exists;
- official audio has fallback links;
- official audio buttons do not scroll or move the page;
- the result is delivered as a downloadable `.html` file.

## Suggested file organization in the repository

Use the existing `/docs` layout described above.

When adding a new exercise or game:

- put the standalone HTML file in `docs/exercises/` or `docs/games/`;
- register it in `docs/index.html` if it is public;
- register it in `docs/studio-k9m3.html` if the user wants studio/private access;
- register it in both if it is public but should also appear in the studio hub.

The final generated page itself must remain standalone: all CSS and JS inline.

## Coding style preference

Keep the implementation simple, localized, and low risk.

Prefer:

- plain HTML, CSS, and JavaScript;
- reusable small functions;
- no dependencies;
- readable markup;
- minimal abstractions;
- changes that are easy to review.

Avoid:

- frameworks;
- build systems;
- complex state management;
- hidden generated behavior that makes the page hard to maintain;
- overengineering.

## Final instruction

Whenever you generate or update a PULS Dansk DU3 study HTML file, prioritize the learner experience: accurate Danish, clear layout, tappable TTS, visible answers, mobile usability, and faithful reproduction of the relevant book sections.