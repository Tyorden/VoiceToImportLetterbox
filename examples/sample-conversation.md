# Sample Conversation - Conversational Mode

This shows how a typical movie logging session might flow.

---

**AI:** Hey! What have you been watching lately?

**User:** Oh man, I finally caught up on some stuff. Saw Dune Part Two in IMAX last week, it was incredible.

**AI:** Nice! Dune: Part Two (2024) in IMAX sounds like the way to see it. What would you rate it? And do you remember which day last week?

**User:** Definitely a 5. I think it was Saturday... the 23rd?

**AI:** Got it - Dune: Part Two, 5 stars, November 23rd, IMAX. What else have you been watching?

**User:** Watched that Denzel train movie on a flight

**AI:** Which Denzel train movie - Unstoppable (2010) or The Taking of Pelham 123 (2009)?

**User:** The runaway train one

**AI:** That's Unstoppable (2010). Rating? And do you remember when the flight was?

**User:** Maybe a 3.5? Flight was sometime early November, don't remember exactly

**AI:** I'll leave the date blank then. Unstoppable, 3.5 stars, watched on a flight. Anything else?

**User:** Yeah watched Past Lives with my girlfriend, we both loved it

**AI:** Past Lives (2023) - that's a beautiful one. Rating?

**User:** 4.5 for sure

**AI:** Got it. Any specific date, or should I leave that blank?

**User:** Hmm maybe like 2 weeks ago? Let's just leave it blank

**AI:** No problem. So far I have:
1. Dune: Part Two (2024) - 5 stars - Nov 23 - IMAX
2. Unstoppable (2010) - 3.5 stars - flight
3. Past Lives (2023) - 4.5 stars - date night

Anything else to add?

**User:** Oh I rewatched The Dark Knight too, just had it on in the background

**AI:** The Dark Knight (2008) - classic. Any rating or was it more background viewing?

**User:** Nah no rating, it was just on

**AI:** Got it. So the final list:
1. Dune: Part Two (2024) - 5 stars - Nov 23 - IMAX
2. Unstoppable (2010) - 3.5 stars - flight
3. Past Lives (2023) - 4.5 stars - date night
4. The Dark Knight (2008) - no rating - rewatch

Ready to generate the CSV?

**User:** Yep!

**AI:** Here's your Letterboxd import file:

```csv
Title,Year,Rating,WatchedDate,Rewatch,Tags,Review
Dune: Part Two,2024,5,2024-11-23,false,"imax",
Unstoppable,2010,3.5,,false,"flight",
Past Lives,2023,4.5,,false,"date night",
The Dark Knight,2008,,,true,,
```

Copy this and import at letterboxd.com/import!

---

## Key Behaviors Demonstrated

1. **Clarifying ambiguous titles** - Asked which Denzel train movie
2. **Flexible on dates** - Left blank when user wasn't sure
3. **Capturing context as tags** - IMAX, flight, date night
4. **Handling rewatches** - Noted The Dark Knight as a rewatch
5. **Optional ratings** - Didn't push for a rating on background viewing
6. **Periodic summaries** - Confirmed the list before generating CSV
