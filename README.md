# Letterboxd Voice Logger

**Log movies through conversation, not clicking.**

A collection of AI prompts that let you naturally talk about movies you've watched and output perfectly formatted CSV files for Letterboxd's bulk import.

## Why?

Adding movies to Letterboxd one-by-one is tedious. Their API isn't available for personal projects. But they fully support CSV import—so this bridges the gap.

Just paste a prompt into ChatGPT, Claude, or any AI assistant. Have a conversation. Get a CSV. Import to Letterboxd. Done.

## The app

**Open `app.html`** (same host as this site, e.g. `/app.html`) — the prompts above, as a tool:

- **Hold the mic** (or Space) and talk, or type. "Inception five stars, rewatch. Dune, the 2021 one, four and a half. Past Lives last Friday on a flight."
- Films land in a **staging table** with clickable stars, watched date, tags, review, rewatch.
- Add a free **TMDB API key** in settings (optional) to verify years, show posters, catch remakes ("which Suspiria?"), and export exact `tmdbID`s. Without it, films export as Title + Year and Letterboxd fuzzy-matches.
- Say **"done"** for the review screen: validation warnings, live CSV preview, one-click download (auto-splits over 1 MB).
- Voice commands: `undo`, `wrong one`, `clear`, `review: <text>` (adds a review to the last film), `both five` (rates every film in the sentence).
- Everything stays in your browser (localStorage). No backend.

Three modes match the prompts: **Quick Dump** (no date prompts), **Conversational** (follow-ups), **Detailed Diary** (dates each film today unless you say otherwise, nudges for ratings/reviews).

Tip: separate films with periods or "and then". A title containing a comma (e.g. *Paris, Texas*) is best typed with the year: "Paris, Texas 1984".

## Quick Start (prompt version)

1. **Copy a prompt** from the `/prompts` folder
2. **Paste** into your AI tool of choice
3. **Talk about movies** you've watched
4. **Copy the CSV output** when done
5. **Import to Letterboxd** at [letterboxd.com/import](https://letterboxd.com/import/)

## Prompts

| Prompt | Best For |
|--------|----------|
| [Quick Dump](prompts/QUICK_DUMP.md) | Rapidly logging a backlog of films |
| [Conversational](prompts/CONVERSATIONAL.md) | Natural recall, casual logging |
| [Detailed Diary](prompts/DETAILED_DIARY.md) | Full entries with dates, reviews, tags |

## How Letterboxd Import Works

Upload a CSV with any of these columns:
- `Title` + `Year` (fuzzy matching)
- `tmdbID` or `imdbID` (exact matching)
- `Rating` (0.5-5 scale)
- `WatchedDate` (creates diary entry)
- `Tags`, `Review`, `Rewatch` (optional extras)

See [full format docs](docs/LETTERBOXD_FORMAT.md) for details.

## Tips

- For best results, the AI will look up release years to improve matching
- If a title is ambiguous (multiple versions/remakes), the prompt asks for clarification
- You can include TMDB IDs if you want exact matching
- Max file size is 1MB—split if needed (keep header row in each file)

## Examples

Check the [examples folder](examples/) for:
- [Sample conversation](examples/sample-conversation.md) - See how a logging session flows
- [Sample output](examples/sample-output.csv) - Basic CSV output
- [Sample diary output](examples/sample-output-diary.csv) - Full diary mode with dates/reviews

## Design

A voice-first app version is being designed. See [`design/`](design/) for the mockups, iterations, and design tokens, and [`docs/DESIGN_BRIEF.md`](docs/DESIGN_BRIEF.md) for the brief.

## Contributing

Ideas for new prompt variants? Found a bug in the CSV formatting? PRs welcome.

## License

MIT

---

*This is a community tool, not affiliated with Letterboxd. They encourage helper scripts for their importer—[see their docs](https://letterboxd.com/about/importing-data/).*
