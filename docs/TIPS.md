# Tips & Troubleshooting

## Best Practices

### For Better Matching

- **Include the year** - Helps distinguish remakes and films with similar titles
- **Use official titles** - "The Godfather" not "Godfather"
- **Check spelling** - The AI should catch this, but typos can cause mismatches
- **Foreign films** - Use the English title that Letterboxd uses (usually matches TMDB)

### For Easier Logging

- **Don't force ratings** - It's fine to leave ratings blank
- **Approximate dates are okay** - Better to leave blank than guess wrong
- **Use tags freely** - theater, streaming service, who you watched with, mood
- **Keep reviews short** - 1-3 sentences capture the essence

### For Large Backlogs

- Break into multiple sessions (the AI's context is limited)
- Group by time period: "Let's do everything from November"
- Or by source: "Let's log everything I watched on Netflix"
- Remember the 1MB file limit - split if needed

## Common Issues

### "Film not found on import"

The import preview shows films Letterboxd couldn't match. This usually means:
- Title spelling doesn't match Letterboxd's database
- It's a TV show or miniseries (not supported)
- It's a very obscure film not in TMDB

**Fix:** Edit the CSV to use the exact title from Letterboxd, or add the tmdbID/imdbID for exact matching.

### Duplicate Entries

If you import the same film with the same WatchedDate, they merge. If these are legitimately separate viewings (double feature, etc.), add them manually on Letterboxd.

### Wrong Version Matched

For films with multiple versions (Dune, A Star Is Born, etc.), add the Year column to help matching. If still wrong, use the tmdbID for exact matching.

### Special Characters in Titles

Titles with commas, quotes, or special characters:
- Commas: Wrap in quotes - `"Paris, Texas"`
- Quotes in title: Escape with backslash - `"She said \"Hello\""`
- Accented characters: Should work fine with UTF-8 encoding

### Large File Won't Upload

Letterboxd has a 1MB file limit. Split your file:
1. Keep the header row in every file
2. Import each file separately
3. Works better in Chrome

## Getting Exact Matches

If fuzzy title matching isn't working, you can use exact IDs:

### Finding TMDB IDs
1. Go to [themoviedb.org](https://www.themoviedb.org/)
2. Search for the film
3. The ID is in the URL: `themoviedb.org/movie/693134` -> `693134`

### Finding IMDb IDs
1. Go to [imdb.com](https://www.imdb.com/)
2. Search for the film
3. The ID is in the URL: `imdb.com/title/tt1160419` -> `tt1160419`

### Using IDs in CSV

```csv
tmdbID,Rating,WatchedDate
693134,5,2024-11-25
```

Or:

```csv
imdbID,Rating,WatchedDate
tt1160419,5,2024-11-25
```

## Rating Conversions

If you think in different scales:

| Your Scale | Letterboxd Scale |
|------------|------------------|
| 10/10 | 5 |
| 9/10 | 4.5 |
| 8/10 | 4 |
| 7/10 | 3.5 |
| 6/10 | 3 |
| 5/10 | 2.5 |
| 4/10 | 2 |
| 3/10 | 1.5 |
| 2/10 | 1 |
| 1/10 | 0.5 |

The prompts handle this conversion automatically if you mention ratings like "8/10" or "8 out of 10".

## After Import

- Check your diary to make sure entries look correct
- Films import as "watched" - adjust dates if needed
- You can edit individual entries on Letterboxd after import
- There's no undo for bulk imports, so review the preview carefully!
