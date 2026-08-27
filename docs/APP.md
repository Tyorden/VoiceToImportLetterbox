# Voice Logger App (`app.html`)

A single static HTML file — no build step, no backend. Deploys with the site on Vercel.

## Using it

1. Open `/app.html` (serve over http/https; the mic does not work from `file://`).
2. Pick a mode: **Quick Dump** (no date prompts), **Conversational** (asks follow-ups), **Detailed Diary** (dates each film today unless you say otherwise; nudges for ratings/reviews).
3. Hold the mic button or **Space** and talk, or type in the box. Examples that parse:
   - `Inception five stars, rewatch`
   - `Dune, the 2021 one, four and a half`
   - `Past Lives last Friday on a flight "quiet and devastating"`
   - `Heat, the original, no rating`
   - `Barbie and then Oppenheimer, both five`
4. Fix anything flagged in the table (every cell is editable; click stars to rate, click the same star again for a half).
5. Say **done** (or click *Review & export*), check the warnings and CSV preview, click **Download CSV**, then upload at [letterboxd.com/import](https://letterboxd.com/import/).

### Rapid rate
Click **Rapid rate** (or say *rate them*). Every film without a rating comes up one at a time as a card — poster, title, year. Tap a star (or press **1–5**; **Shift+number** or **½ mode** for halves), toggle **Rewatch** (**R**), or **Skip** (**S** / →). The next card slides in. If everything is already rated it walks through all films so you can re-score. **Esc** returns to the session.

### Voice/text commands
| Say | Does |
|-----|------|
| `done` / `export` / `review` | Opens the review screen |
| `undo` / `scratch that` | Removes the last film |
| `wrong one` | Re-opens version choices for the last matched film (needs TMDB key) |
| `four and a half` (alone) | Rates the last film |
| `review: <text>` | Adds a review to the last film |
| `clear` / `start over` | Empties the table |
| `1977` / `the original` / `the remake` / `one` / `two` / `neither` | Answers a "which version?" question |
| `rate them` / `rapid rate` | Opens Rapid rate |
| `help` | Shows a hint |

### Optional: TMDB lookup
Settings (gear icon) → paste a free [TMDB v3 API key](https://www.themoviedb.org/settings/api). The key is kept in `localStorage` only and sent straight from your browser to TMDB. With it, the app:
- confirms years and canonical titles, shows posters
- flags titles with several well-known versions (`NEEDS REVIEW`) and shows an inline chooser
- exports `tmdbID` so Letterboxd matches exactly

Without it, rows show `TITLE ONLY` and export as Title + Year (Letterboxd fuzzy-matches).

## What it parses
- **Rating**: `5`, `4.5`, `five stars`, `four and a half`, `gave it a 4`, `rated 3`, trailing `, four`; `both/all five` applies to every film in the sentence
- **Rewatch**: `rewatch`, `rewatched`
- **Year**: any 4-digit year; `the 2021 one`; `the original` / `the remake` pick oldest/newest match
- **Date** → `WatchedDate`: `yesterday`, `last night`, `last Friday`, `on Aug 20th`, `8/20`, ISO dates
- **Tags**: `in theaters`, `imax`, `on a flight`, `on Netflix/Max/Criterion/…`, `date night`, `tag <word>`
- **Review**: anything in quotes
- **Splitting**: periods, `and then`, `also`, and commas followed by a new title (commas followed by a modifier like a rating or year stay with the current film)

## Export rules (see `LETTERBOXD_FORMAT.md`)
Columns are included only when some row has data (`Title,Year` always). Fields with commas/quotes are quoted; quotes escaped as `\"`. Empty optionals are blank. `Rewatch` is only written when a `WatchedDate` exists. Files over ~1 MB are split, each with a header row.

## Known limitations
- Titles containing a comma (*Paris, Texas*) split into two rows — type them with the year, or fix in the table.
- Speech recognition requires Chrome, Edge, or Safari; Firefox is text-only.
- Director isn't fetched (would need a second TMDB call per film); the chooser shows year + poster.
- No account sync — the session lives in this browser's `localStorage`.

## Design
Implements the lead direction in `design/iterations/` (`Main`, `MobileSession`, `ReviewExport` artboards). Tokens are listed in `design/README.md`.
