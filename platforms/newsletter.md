# Newsletter — format rules

## Output shape
Produce **one newsletter section, 180-260 words**, suitable to drop into a weekly email digest alongside other items.

Output sections:
- `## SUBJECT LINE` (one option, <50 chars)
- `## PREVIEW TEXT` (under 90 chars, complements — doesn't repeat — subject)
- `## SECTION BODY`

## Section body structure
1. **Opening line** — stat or concrete claim, no "We are excited to announce"
2. **Context paragraph** — 2-3 sentences on what shipped and why it matters
3. **Bulleted highlights** — 3-5 bullets, each a feature + one-line benefit
4. **Closing line with link** — "Full release →" with UTM `utm_source=newsletter&utm_medium=email&utm_campaign=<slug>`

## Voice patterns
- Digest tone — informed friend, not marketing email
- Treat the reader like they already know what Shopsys is
- Inline link markup: `[label](url)`

## Anti-patterns
- Emoji in subject line (universally reads as spam)
- "Dear subscriber" / "Hi there" — omit greeting entirely
- Stacking every feature from the release — pick 3-5 most interesting to this audience
- P.S. blocks pitching unrelated things

## Subject line patterns that work
- "{Product} {version}: {one concrete thing}"
- "{Number} + what it means for {audience}"
- Question format: "Why we removed 12k lines of admin code"
