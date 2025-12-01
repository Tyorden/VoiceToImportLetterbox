# Letterboxd Voice Logger

**Log movies through conversation, not clicking.**

A collection of AI prompts that let you naturally talk about movies you've watched and output perfectly formatted CSV files for Letterboxd's bulk import.

## Why?

Adding movies to Letterboxd one-by-one is tedious. Their API isn't available for personal projects. But they fully support CSV import—so this bridges the gap.

Just paste a prompt into ChatGPT, Claude, or any AI assistant. Have a conversation. Get a CSV. Import to Letterboxd. Done.

## Quick Start

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

## Contributing

Ideas for new prompt variants? Found a bug in the CSV formatting? PRs welcome.

## License

MIT

---

*This is a community tool, not affiliated with Letterboxd. They encourage helper scripts for their importer—[see their docs](https://letterboxd.com/about/importing-data/).*
