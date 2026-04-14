# Dev.to / Hashnode — format rules

## Output shape
Produce **one full technical article, 500-900 words**, developer-audience tone.

Output sections:
- `## TITLE` (one line, ≤ 70 chars)
- `## TAGS` (3-4 dev.to tags, e.g. `symfony, php, ecommerce, webdev`)
- `## COVER BLURB` (1-2 sentences, the preview)
- `## ARTICLE BODY` (markdown — code blocks, headings, lists welcome)

## Structural rules
- **Title:** specific + dev-focused. Not the marketing title of the blog post. Pick an angle a dev would click.
  - Good: "Replacing DateTime with Symfony Clock in a large Symfony app"
  - Bad: "Shopsys 18.0 Release Highlights"
- **Intro:** problem framing, 2-4 sentences, end with "Here's what we did."
- **Body:** H2 sections. Include at least one code block if the blog mentions any technical change (can be illustrative — not exact repo code, but representative).
- **Close:** "Takeaways" list + link to GitHub release with UTM `utm_source=devto&utm_medium=organic&utm_campaign=<slug>`.

## Voice patterns
- Engineer-to-engineer. No marketing filler
- Show your work. Explain *why*, not just *what*
- Tradeoffs acknowledged. Don't pretend the choice was free
- "We" framing, Shopsys team speaking

## Anti-patterns
- Generic listicles covering every feature — pick ONE technical angle and go deep
- "You should try Shopsys" at the bottom — the article speaks for itself, link is enough
- Code blocks with fake / placeholder code that doesn't compile

## Code block conventions
- Real language tag (```php, ```graphql, ```yaml, ```sh)
- Before/after pairs where relevant
- Comments explaining the non-obvious, not restating the line
