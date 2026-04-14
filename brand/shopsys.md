# Shopsys — brand voice & positioning

## Who we are
Open-source, developer-centric e-commerce platform built on Symfony. B2B and B2C. Modular, headless, enterprise-grade. Competitors: Magento (Adobe Commerce), Shopware, Sylius. Target customer: mid-to-enterprise retailers with real engineering teams.

## Who we talk to
- **Primary:** ecommerce CTOs, platform leads, senior backend engineers (PHP/Symfony ecosystem)
- **Secondary:** head of digital / ecommerce director (ROI angle, time-to-market)
- **Tertiary:** ecommerce developers evaluating frameworks

## Voice

Pragmatic, technical, confident, not salesy. We sound like engineers writing to engineers. Credibility comes from specifics — version numbers, API names, concrete line counts, measured percentages. Not from adjectives.

Tone slider: 80% pragmatic-technical, 20% opinionated-contrarian. We are not afraid to say "this replaces legacy custom theming" or "we removed 12,000 lines of legacy code." Honesty about tradeoffs reads as expertise.

## Vocabulary

**Use:** platform, storefront, admin, merchant, domain, locale, release, version, feature, GraphQL, Symfony, async, payload, checkout critical path, conformance, locale-specific collation, multi-domain, headless.

**Avoid:** game-changer, revolutionize, unleash, synergy, cutting-edge, world-class, seamless (used once max), next-gen, solution (as filler), ecosystem (as filler).

## Always / never

**Always:**
- Ground claims in specifics (version 18.0.0, not "the latest version"; 70-80% reduction, not "massive reduction")
- Credit contributors / community when relevant
- Link to docs / release notes / GitHub for proof
- Treat the reader as smarter than the average marketing post assumes
- When talking developer features, show the "before/after" framing

**Never:**
- Emoji spam (max 1 emoji per post, and only when it clarifies; zero emoji is better than forced ones)
- Hype without proof
- Marketing-speak abstractions ("empower", "elevate", "drive value")
- Pretend features are bigger than they are — a 12k line cleanup is significant, don't inflate it to "transformation"
- Hashtag stuffing — 1-3 max, relevant only

## Mode map (how voice shifts per channel)

| Channel | Mode | Example hook |
|---|---|---|
| LinkedIn | Pragmatic-opinionated, first person plural "we" | "We removed 12,000 lines of legacy admin code in 18.0. Here's what replaced it." |
| Twitter/X | Terse, punchy, thread format, one idea per tweet | "Shopsys 18.0 shipped. The highlight nobody will talk about: -12,000 lines of admin." |
| Instagram | Visual, carousel, stat-first, fewer words | Slide 1: "-12,000 lines" / Slide 2: "why that matters" |
| Shorts/Reels | Hook in 3 seconds, one idea, explicit CTA | "Your admin is probably 12,000 lines heavier than it needs to be." |
| Newsletter | Digest tone, 2-3 picks, inline links | "This release: admin overhaul, QR payments, -70% payload on product detail." |
| Dev.to / Hashnode | Technical depth, code block friendly, longer form | Title: "Replacing DateTime with Symfony Clock in a large Symfony codebase — what we learned in Shopsys 18.0" |

## Proof library (ground every claim in this)

- **12,000 lines** of legacy admin code removed (Tabler UI migration, v18)
- **70-80% payload reduction** on product detail GraphQL (v18)
- **Symfony Clock** integration eliminates timing-related test flakiness (v18)
- **Asynchronous order email** moved out of checkout critical path (v18)
- Open source on GitHub (`shopsys/shopsys`)

## CTA preferences

In priority order: (1) GitHub release page, (2) docs.shopsys.com, (3) shopsys.com/shopsys-platform, (4) GitHub Discussions. Never generic "contact sales" — we're a platform, not a funnel.

## UTM convention

All outbound links use: `?utm_source=<channel>&utm_medium=organic&utm_campaign=<post-slug>`. Channel values: `linkedin`, `twitter`, `instagram`, `shorts`, `newsletter`, `devto`.
