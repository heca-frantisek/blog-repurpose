# Twitter / X — format rules

## Output shape
Produce **one thread of 5-9 tweets**. Format as `1/`, `2/`, ... prefixes. End with explicit CTA tweet.

## Structural rules
- **Per-tweet character count:** ≤ 280 chars including the `N/` prefix and any URL
- **Tweet 1 (hook):** strongest single-line stat or contrarian claim. Optionally end with "🧵" if it reads naturally, else skip
- **Middle tweets:** one idea per tweet. No continuation across tweets with "..." — each should stand on its own enough
- **Penultimate tweet:** the takeaway or lesson
- **Final tweet:** CTA + link with UTM `utm_source=twitter&utm_medium=organic&utm_campaign=<slug>`

## Voice patterns
- Terse. Cut every filler word
- Short sentences. Periods over commas
- Specific numbers
- Occasional bold claim to drive engagement
- Lowercase often reads more native on X, but don't force it — use whatever matches the content's energy

## Anti-patterns
- Threads longer than 9 tweets — if you have more material, make 2 threads
- Per-tweet hashtag stuffing (use 0-1 hashtag in the whole thread, not per tweet)
- "Subscribe / follow for more" filler
- Link in every tweet — one link in the CTA tweet only
- Em-dash overload (X crowd is trained to sniff these out as AI)

## Example (reference for voice only)

```
1/ Shopsys 18.0 shipped.

Headline feature nobody's talking about: we deleted 12,000 lines of admin code.

2/ The old admin was a decade of bespoke theming.

Every merchant had to wrangle it. Every new dev had to learn it.

3/ Replaced with Tabler UI. One convention. Mobile works out of the box. Modals don't fight you.

4/ This is the boring, unglamorous kind of release that makes a platform actually usable for the next five years.

5/ Less code → less surface area → fewer bugs → shorter onboarding.

The best features are the ones you remove.

6/ Full release notes: github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=twitter&utm_medium=organic&utm_campaign=release-highlights-18-0-0
```
