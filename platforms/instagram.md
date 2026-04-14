# Instagram Carousel — format rules

## Output shape
Produce **one 5-slide carousel** plus the caption.

Output sections:
- `## SLIDE 1 — Hook` ... through `## SLIDE 5 — CTA`
- `## CAPTION`

## Slot structure (fixed — subagents MUST use these 5 slots)

| Slot | Purpose | Word count |
|---|---|---|
| Slide 1 | **Hook** — stat or question that stops scroll | ≤ 12 words |
| Slide 2 | **Stat/Claim** — the specific number or bold claim | ≤ 18 words |
| Slide 3 | **Value** — why this matters to the audience | ≤ 30 words |
| Slide 4 | **Proof** — concrete before/after or feature name | ≤ 30 words |
| Slide 5 | **CTA** — one line + "link in bio" | ≤ 15 words |

## Visual direction (include these notes for the designer as `[visual: ...]` after each slide text)
- Slide 1: bold type-forward, background solid brand color
- Slide 2: giant numeral treatment
- Slide 3: 3-icon row or 1 illustration
- Slide 4: screenshot / code snippet / product UI
- Slide 5: logo lockup + arrow

## Caption rules
- 1-2 short paragraphs, max 300 chars
- "Full release notes" phrase + link (UTM: `utm_source=instagram&utm_medium=organic&utm_campaign=<slug>`)
- 5-8 hashtags at the end, relevant mix: `#ecommerce #symfony #opensource #webdev #shopsys #php #devex #b2b`

## Anti-patterns
- More than 5 slides (our format is fixed at 5 for consistency across the grid)
- Full sentences on Slide 1 or Slide 2 — these are designed to be skimmed at scroll speed
- Generic "swipe →" prompts — let the visual do that work
