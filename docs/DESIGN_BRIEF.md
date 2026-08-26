# Design Brief — Letterboxd Voice Logger

Paste this into Claude Design (or any design tool/AI) with no other context.

---

Design a web app called **Letterboxd Voice Logger**. It lets people log the movies they've watched by *talking* (or typing conversationally) instead of clicking through forms, and turns that conversation into a CSV file that Letterboxd's bulk importer accepts.

## Context
- Letterboxd has no public API for personal projects, but it supports CSV import with these columns: `Title`, `Year`, `tmdbID`/`imdbID` (optional, exact match), `Rating` (0.5–5, half-stars), `WatchedDate` (YYYY-MM-DD, creates a diary entry), `Tags`, `Review`, `Rewatch` (true/false). Max file 1 MB.
- Today the project is just a set of text prompts users paste into ChatGPT/Claude. We want a real, purpose-built tool that keeps the conversational feel but makes the output trustworthy and effortless.
- Audience: film enthusiasts with a backlog of 10–500 films to log. They care about accuracy (right film, right year, no remake mix-ups) and speed.

## Three logging modes (must be visible and switchable)
1. **Quick Dump** — rapid-fire: user rattles off titles + ratings, minimal back-and-forth.
2. **Conversational** — natural recall ("I watched that Nolan one with the spinning top last week, loved it").
3. **Detailed Diary** — full entries with watched dates, reviews, tags, rewatch flag.

## Core screens to design
1. **Session screen** — split layout: a chat/voice transcript on one side (with a prominent push-to-talk/mic button and a text fallback), and a live-updating **staging table** of parsed films on the other. Each row: poster thumbnail, title, year, rating (star widget), watched date, tags, review snippet, rewatch toggle, and a match-confidence indicator.
2. **Disambiguation state** — when a title is ambiguous (e.g., "Dune" 1984 vs 2021, "Suspiria"), show an inline chooser with posters/years so the user can resolve it without breaking flow.
3. **Review & export screen** — full editable table, validation warnings (missing year, rating out of range, file over 1 MB → auto-split into multiple CSVs with header rows), a CSV preview, and a big "Download CSV" + "Open Letterboxd Import" CTA.
4. **Empty/onboarding state** — explains the flow in three steps: Talk → Review → Import.
5. **Mobile layout** for the session screen (voice-first, table collapses into cards).

## Design direction
- Dark, cinematic, but not a Letterboxd clone — avoid its exact green/orange/blue palette and logo. Feel: a comfortable late-night movie-logging companion.
- Voice is the hero: the mic button and transcript should feel alive (waveform/listening states, "thinking" state, error state when a film can't be found).
- Prioritize clarity of the staging table — users must be able to spot a wrong match at a glance.
- Include a small component sheet: star-rating control, confidence badge (exact / fuzzy / needs review), film row, mode switcher, mic states.

Deliver desktop artboards for screens 1–4, one mobile artboard for the session screen, and the component sheet.
