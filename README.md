# Rhetorical Modes — Slidev Deck

An instructional Slidev presentation for **Academic Reading and Writing**,
covering the nine rhetorical modes from *Writing for Success* (LibreTexts).
26 slides, fully sourced with an MLA Works Cited slide, original inline-SVG
iconography (no external image assets), and per-slide speaker notes written
as narration script — ready for both a live Slidev deployment and
[deck2video](https://github.com/pjdoland/deck2video) narrated-video export.

## Quick start (regular Slidev)

```bash
npm install
npm run dev       # live-reload editor at http://localhost:3030
npm run build      # static site -> dist/
npm run export      # PDF export -> slides-export.pdf
npm run export:png  # one PNG per slide -> slides-export/
```

If Playwright's bundled Chromium can't download in your environment, export
with an explicit browser, e.g.:

```bash
npx slidev export slides.md --format pdf --executable-path /path/to/chrome
```

## Deploying to Vercel

The included `vercel.json` is what makes this work — Slidev builds a
client-side-routed single-page app (each slide is a route like `/2`, `/3`, …),
and Vercel needs to be told to serve `index.html` for every path, plus build
with an absolute base path:

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

Two things that cause a 404 on Vercel specifically if either is missing:

1. **Absolute base path.** `npm run build` runs `slidev build slides.md --base /`.
   A relative base (`--base ./`) makes every asset URL relative to whatever
   slide route the browser is on, so anything past the very first slide fails
   to load its JS/CSS.
2. **SPA fallback via `rewrites`.** Slidev's own build emits a Netlify-style
   `_redirects` file for this, but Vercel doesn't read that file — it needs
   the `rewrites` rule above in `vercel.json`, which this project already has.

To deploy: import this folder as a Vercel project (or drag-and-drop deploy),
and in Project Settings confirm Build Command `npm run build` and Output
Directory `dist` (the `vercel.json` sets both automatically, but double-check
if you changed the framework preset). No further configuration is needed.

## deck2video (narrated MP4)

Every slide's speaker notes are written as an HTML-comment narration script
(`<!-- ... -->`), matching deck2video's expected format, with `[click]`
markers on the few slides that use `v-click` reveals (the mode overview
grid, the "What Is a Rhetorical Mode?" cards, and the comparison/contrast
diagram) so narration stays in sync with each animation step.

```bash
pip install -r requirements.txt   # per the deck2video project
python -m deck2video slides.md --format slidev --tts-engine chatterbox \
  --output rhetorical-modes.mp4
```

See the deck2video README for TTS engine setup (Chatterbox local or
ElevenLabs) and additional flags (`--voice`, `--interactive`, `--redo-slides`).

## Project structure

```
slides.md      — the deck (frontmatter + 26 slides + speaker notes)
style.css       — academic theme: palette, type system, component styles
package.json    — Slidev CLI + export scripts
```

## Regenerating the deck from source content

`slides.md` is generated from a shared Python content model so the Slidev
deck and the companion PPTX deck never drift out of sync. If you edit
`../content.py`, `../icons.py`, or `../generate_slidev.py`, re-run:

```bash
python3 ../generate_slidev.py
```

## Sourcing

Content is adapted from *Writing for Success* (LibreTexts), licensed
CC BY-NC-SA 3.0. Full MLA citation appears on the deck's final content
slide ("Works Cited"). Typefaces (Fraunces, Source Serif 4, JetBrains Mono)
are open-source Google Fonts under the SIL Open Font License. All diagrams
and icons are original vector artwork created for this deck.
