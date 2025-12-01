# Letterboxd CSV Import Format

Complete reference for the Letterboxd import format.

## Supported Import Methods

1. **Direct to account** - as watched films and/or diary entries (with optional ratings, reviews, tags)
2. **To a list** - new or existing
3. **To watchlist**

## CSV Requirements

- **Encoding:** UTF-8
- **Delimiter:** Comma (,) - no space after commas
- **Quoting:** Strings containing commas must be quoted: `"Paris, Texas"`
- **Escaping:** Quotes within quoted text use backslash: `\"`
- **File size limit:** 1MB (split larger files, include header row in each)

## Column Definitions

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `LetterboxdURI` | String | Optional | Exact match by Letterboxd URI, e.g., `https://boxd.it/29qU` |
| `tmdbID` | Number | Optional | Exact match by TMDB ID, e.g., `860` |
| `imdbID` | String | Optional | Exact match by IMDb ID, e.g., `tt0086567` |
| `Title` | String | Optional* | Used for fuzzy matching when no ID provided |
| `Year` | YYYY | Optional | Improves title matching accuracy |
| `Directors` | String | Optional | Comma-delimited within quotes, improves matching |
| `Rating` | Decimal | Optional | 0.5-5 scale, supports 0.5 increments |
| `Rating10` | Integer | Optional | 1-10 scale, converted to 0.5-5 |
| `WatchedDate` | YYYY-MM-DD | Optional | Creates diary entry on this date |
| `Rewatch` | Boolean | Optional | Flags as rewatch (requires WatchedDate) |
| `Tags` | String | Optional | Comma-delimited within quotes (requires WatchedDate) |
| `Review` | Text/HTML | Optional | Review text, supports same HTML as Letterboxd site |

**At least one of the first four columns (LetterboxdURI, tmdbID, imdbID, Title) is required.*

## Matching Priority

1. `LetterboxdURI` - exact match
2. `tmdbID` - exact match
3. `imdbID` - exact match
4. `Title` + `Year` + `Directors` - fuzzy/best-guess match

## Example Files

### Minimal (Title + Year + Rating)

```csv
Title,Year,Rating
Top Gun,1986,5
Gremlins,1984,4.5
"Paris, Texas",1984,4
```

### Full Diary Entry

```csv
Title,Year,Rating,WatchedDate,Rewatch,Tags,Review
Dune: Part Two,2024,5,2024-11-25,false,"imax, theater",Villeneuve delivers again
When Harry Met Sally,1989,4.5,2024-11-20,true,"comfort rewatch",Classic
```

### With TMDB IDs (Exact Matching)

```csv
tmdbID,Rating,WatchedDate
693134,5,2024-11-25
639,4.5,2024-11-20
```

## Import Behaviors

- All imported films are auto-marked as watched
- Duplicate film + WatchedDate combinations merge into single entry
- Importing to list: new films append at end, existing films stay in place
- If both Rating and Rating10 have data, the second column in the file takes precedence
- **No undo after confirmation step**

## Tips

- Works best in Chrome for large files
- Multiple lines with same film + same date will combine into single entry
- TV entries may match to similarly-named films - review the preview before confirming
- The importer shows a summary before completing, so you can fix mismatches

## Sources

- [Letterboxd Import Page](https://letterboxd.com/import/)
- [Letterboxd Import Documentation](https://letterboxd.com/about/importing-data/)
