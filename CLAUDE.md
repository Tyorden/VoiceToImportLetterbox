# Letterboxd Voice Logger

Log movies by talking instead of clicking, export a Letterboxd-ready CSV. Two things live here:

1. **The prompts** (`prompts/*.md`) — the original product. Paste one into any AI chat, talk about movies, get a CSV. Shown on `index.html`.
2. **The app** (`app.html`) — a real, static, no-backend implementation of the same idea: mic or text input, a live staging table, a Rapid Rate scoring mode, and CSV export. This is the actively developed piece.

No build step. No framework. No package.json. `index.html` and `app.html` are each a single self-contained file (inline `<style>`, inline `<script>`, no bundler). Deploys to Vercel as static files (`vercel.json` has `buildCommand: null`).

## Orientation for a new session

Read in this order:
- This file, for the shape of things.
- `docs/APP.md` — user-facing app docs: how to use it, voice commands, what the parser understands, TMDB setup, export rules, known limitations. Keep this in sync with `app.html` whenever behavior changes.
- `app.html` itself — ~670 lines, organized under `/* ---------- section ---------- */` comments (parsing, TMDB, transcript, commands+intake, render session, CSV, review view, speech, rapid rate, wiring). It's small enough to read in full; don't try to summarize it from memory.
- `docs/LETTERBOXD_FORMAT.md` — the CSV spec `buildCsv()` in `app.html` must keep matching exactly (column names, quoting/escaping rules, 1MB split behavior).
- `design/README.md` — design tokens and which Claude Design artboards `app.html` implements (`design/iterations/Main.dc.html`, `MobileSession.dc.html`, `ReviewExport.dc.html`). If you're asked to change the app's visual design, check whether the artboards should change too (they're a separate, non-live copy — editing `app.html` does not update them).
- `docs/DESIGN_BRIEF.md` — the original brief the designs were generated from, useful context if extending the design further.

## How app.html works (mental model)

Single global state object `S` (mode, films array, transcript log, TMDB key, tts flag), persisted to `localStorage` on every mutation via `save()`. Each film is a plain object: `{title, year, rating, date, tags, review, rewatch, tmdb, poster, match, candidates}`. `match` is one of `exact | fuzzy | needs | none` and drives the badge shown.

Flow: raw text (typed or from Web Speech API) → `handle(text)` → checks for commands (`done`, `undo`, `wrong one`, disambiguation answers, bare rating follow-ups) → falls through to `splitChunks(text)` (splits multi-film utterances on sentence breaks / "and then" / commas-before-a-new-title) → `parseFilm(chunk)` per chunk (regex-based: pulls rating, date, tags, rewatch, year, quoted review, then title-cases whatever's left) → pushed into `S.films` → if a TMDB key is set, `resolve(f)` looks it up and sets `match`/`poster`/`tmdb`.

The parser is regex-based and deliberately permissive — it's tuned against real phrasings in `docs/APP.md`'s examples. If you change `parseFilm`, `parseRating`, `parseDate`, `parseTags`, or `splitChunks`, re-run the inline test harness below before committing; there's no separate test file.

```bash
node -e '
const src=require("fs").readFileSync("app.html","utf8");
const a=src.indexOf("/* ---------- parsing"), b=src.indexOf("/* ---------- TMDB");
eval(src.slice(a,b));
const tests=[
  "Inception five stars, rewatch. Dune, the 2021 one, four and a half.",
  "Past Lives four and a half, The Thing rewatch five, and Heat.",
  "Heat, the original, no rating",
  "Barbie and then Oppenheimer, both five",
];
for (const t of tests){ console.log("\n> "+t); for (const c of splitChunks(t)) { const f=parseFilm(c); console.log(" ", f.title, f.year, f.rating, f.date, f.tags, f.rewatch?"RW":""); } }
'
```

Also syntax-check the whole inline script after any edit (there's no linter wired up):

```bash
node -e 'const s=require("fs").readFileSync("app.html","utf8"); new Function(s.slice(s.indexOf("<script>")+8, s.lastIndexOf("</script>"))); console.log("ok")'
```

Three screens, one file, toggled by classes on `#app` (`.reviewing`, `.rapid-on`) rather than routing:
- **Session** (default) — transcript + mic dock (left), staging table (right).
- **Review & export** (`openReview()`) — validation warnings, editable table, live CSV preview, `download()` builds the file(s) and triggers browser download(s), auto-splitting over ~1MB per `LETTERBOXD_FORMAT.md`.
- **Rapid rate** (`openRapid()`) — full-screen, one unrated film at a time as a card with big star buttons; keyboard 1–5 (+Shift for halves), R for rewatch, S/→ to skip, Esc to exit.

TMDB lookup is opt-in (settings gear, stores key in `localStorage` only, calls TMDB directly from the browser — no proxy/backend). Without a key everything still works, just marked `TITLE ONLY` and exported as Title+Year for Letterboxd's own fuzzy matching.

## Design system / tokens

CSS custom properties at the top of both `index.html`'s `<style>` and `app.html`'s `<style>` (`--bg`, `--surface`, `--accent` etc. — pink/plum palette, Sora for display type, IBM Plex Sans/Mono for body/data). Keep both files' tokens in sync if you change the palette; full token list is also duplicated in `design/README.md` for reference. Recent contrast pass (commit `61d1b72`) lifted `--muted`/`--faint` and darkened ink on pink buttons — don't regress that.

## Conventions / gotchas learned in this project

- **Git commit email**: commit as `tylerorden@gmail.com`, not whatever the Claude account defaults to — a mismatched email makes Vercel deploys silently stall. (Also in global memory `git-commit-email.md`.)
- GitHub account is `Tyorden`, Vercel account is `tyorden`. The `gh` token available here lacks `workflow` scope — don't rely on it for workflow-file changes.
- Don't rebuild or touch `~/Desktop/sendtojustin/` unless explicitly asked — that was a one-off export folder for sharing outside the repo, not something to keep in sync with every change.
- When incorporating a design (Claude Design canvas, a brief, mockups) the expectation is that it lands in the actual app/site as working code — not just archived under `design/`.
- No automated test suite or CI. Verification so far has been: the Node one-liners above for the parser/syntax, and manual review — the Chrome extension for live browser testing has been unavailable in-session so far (extension not connected), so mic/voice behavior and the Rapid Rate card animation are unverified in a real browser. If Chrome automation is available in a future session, actually exercise `app.html` there before claiming a UI change works.
- macOS filesystem permissions have intermittently blocked shell access to `~/Desktop` in this environment (Full Disk Access / Files and Folders permission for the terminal app can get revoked, e.g. across a system update) — if `ls`/`git status` fail with "Operation not permitted" even with the sandbox disabled, that's the cause; it's a user-side System Settings fix, not something to work around in-session.

## Commit / push

This repo has a remote (`origin` → `github.com/Tyorden/VoiceToImportLetterbox`) and pushes have been going straight to `main` (no PR workflow set up so far). Keep doing that unless asked otherwise. Sign commits with the attribution footer given in-session (varies by session; check the current system reminder rather than copying an old one).
