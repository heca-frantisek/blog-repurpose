# LinkedIn — format rules

## Output shape
Produce **2-3 LinkedIn posts** from the atoms, each exploring a different angle. Each as a separate `## LINKEDIN POST N — <angle>` section.

Angles to pick from (pick distinct ones across the 2-3 posts):
- **Key insight:** lead with the biggest stat or concrete outcome
- **Contrarian:** frame what the industry does wrong, what Shopsys did instead
- **Behind-the-scenes:** developer POV, why a change was made, the tradeoff
- **Merchant-impact:** feature → operator benefit → dollars/hours/errors saved
- **Migration story:** before/after framing for a specific technical change

## Structural rules
- **Hook:** first 2 lines (≤220 chars combined). Must work as preview before the "see more" cutoff. Start with a stat, contrarian claim, or concrete question — never with "I'm excited to announce" or similar.
- **Body:** 80-220 words. Short paragraphs (1-3 lines). Line breaks for scannability. Use `•` or `→` for list points if a list helps.
- **Close:** one explicit CTA line with link (GitHub release, docs, or platform page, with UTM).
- **Hashtags:** 1-3 maximum at the very end, on their own line. Relevant only: `#ecommerce`, `#symfony`, `#opensource`, `#devex`, `#php`.

## Voice patterns
- First person plural ("we shipped", "we removed") — Shopsys speaking
- Specific numbers always beat adjectives
- One idea per post. Don't stack all features — pick ONE angle per post and go deep

## Anti-patterns
- Emoji at start of every line
- "🚀 Excited to share..." openers
- Generic closing like "Let me know your thoughts in the comments!"
- Hashtag walls
- Reposting the blog title as the hook

## Example (not to be copied — reference for voice)

```
We removed 12,000 lines of admin code in Shopsys 18.0.

The new UI isn't fewer features — it's Tabler framework replacing a decade of bespoke theming. Mobile works. Forms are standardized. Modals don't fight you.

What we got back:
→ One convention instead of six
→ Shared components across all admin screens
→ Onboarding time for new devs — days, not weeks

The lesson: legacy isn't what you built, it's what nobody wants to touch. Cutting it is a feature.

Full release → github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=linkedin&utm_medium=organic&utm_campaign=release-highlights-18-0-0

#ecommerce #symfony #opensource
```
