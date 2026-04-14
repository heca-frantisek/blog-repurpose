# PLAN — Blog-to-Multiplatform Pipeline

**Challenge:** Abugo CMO hiring #04 — repurpose a blog post into platform-native content across 6+ channels, using Claude Code as the runtime.

**Operator model:** User opens local Claude Code in this directory and types `/blog-repurpose <url>`. The skill orchestrator does the rest — fetch, decompose, fan-out to per-platform subagents in parallel, validate against conformance rules, Ralph-retry failures, save outputs.

## Design principles (from shopsys/automation-handbook)

- **Deterministic orchestrator + AI steps** (Task 29): skill owns the flow, subagents are specialists
- **Conformance suites as executable spec** (Task 28): YAML rules per platform, mechanical gate — not vibes
- **Intent-driven** (knowledge-base/intent-driven-development.md): brand voice + platform rules = source of truth; subagents follow
- **Fix the system, not the output** (knowledge-base/fix-the-system-not-the-output.md): when an output disappoints, edit `platforms/*.md`, not the generated post
- **Ralph loop on conformance failures** (Task 21): regenerate only failed platforms, max 3 iterations
- **Agent-first design** (Task 31): constrained inputs (atoms.json, intent files) mean subagents can't drift

## Folder tree

```
04-blog-to-multiplatform/
├── PLAN.md                                   # this file
├── USAGE.md                                  # how to invoke
├── .claude/
│   ├── skills/
│   │   └── blog-repurpose/SKILL.md           # orchestrator, user-invoked via /blog-repurpose
│   └── agents/
│       ├── atom-extractor.md                 # blog → atoms.json
│       ├── linkedin-writer.md                # platform voice
│       ├── twitter-writer.md
│       ├── instagram-writer.md
│       ├── shorts-writer.md
│       ├── newsletter-writer.md
│       ├── devto-writer.md
│       └── conformance-checker.md            # validates outputs vs YAML specs
├── brand/
│   └── shopsys.md                            # voice, audience, do/don't, positioning
├── platforms/                                # intent docs: format rules per channel
│   ├── linkedin.md
│   ├── twitter.md
│   ├── instagram.md
│   ├── shorts.md
│   ├── newsletter.md
│   └── devto.md
├── conformance/                              # executable rules
│   ├── linkedin.yaml
│   ├── twitter.yaml
│   ├── instagram.yaml
│   ├── shorts.yaml
│   ├── newsletter.yaml
│   └── devto.yaml
├── source/                                   # raw blog posts cached by slug
│   └── <slug>.md
└── output/<slug>/                            # generated content per post run
    ├── _source.md
    ├── _atoms.json
    ├── _report.md
    ├── linkedin.md
    ├── twitter.md
    ├── instagram.md
    ├── shorts.md
    ├── newsletter.md
    └── devto.md
```

## Execution flow (what /blog-repurpose does)

1. **Parse URL** from `$ARGUMENTS`. Derive slug (last non-empty path segment).
2. **Fetch** URL via WebFetch → write verbatim content to `source/<slug>.md` and `output/<slug>/_source.md`.
3. **Delegate to atom-extractor** (Task subagent). Input: `_source.md` + `brand/shopsys.md`. Output: `_atoms.json` with `{title, core_thesis, stats[], quotes[], features[], stories[], takeaways[], target_audience[], cta_options[]}`.
4. **Fan out platform writers in parallel** — single turn, 6 Task tool calls. Each writer reads `_atoms.json`, `brand/shopsys.md`, its own `platforms/<x>.md`, and writes `output/<slug>/<x>.md`. UTM source per channel baked into any link.
5. **Delegate to conformance-checker**. Reads each output + matching YAML spec. Produces `_report.md` with pass/fail per platform and per rule.
6. **Ralph retry loop** (up to 3 iterations): for each failing platform, re-invoke its writer with the failure report as additional input. Re-run conformance. Stop when all pass or iterations exhausted.
7. **Print summary** to user: slug, outputs, conformance status, path to `_report.md`.

## Atoms schema (_atoms.json)

```json
{
  "title": "string",
  "url": "string",
  "author": "string",
  "date": "YYYY-MM-DD",
  "core_thesis": "one-sentence what this post is about",
  "target_audience": ["ecommerce CTOs", "platform engineers", ...],
  "stats": [{"value": "70-80%", "claim": "payload reduction on product detail GraphQL", "context": "..."}],
  "quotes": [{"text": "...", "context": "..."}],
  "features": [{"name": "...", "benefit_line": "...", "proof": "..."}],
  "stories": [{"hook": "...", "tension": "...", "resolution": "..."}],
  "takeaways": ["dev-focused", "merchant-focused", "business-focused"],
  "cta_options": ["github release", "docs", "platform page"]
}
```

Atoms are the contract between blog and platforms. Everything downstream consumes this.

## Conformance rule shape (YAML)

Each platform YAML defines mechanical checks. `conformance-checker` walks them.

```yaml
platform: linkedin
rules:
  - id: post-count
    check: count_sections_matching "^## LINKEDIN POST"
    min: 2
    max: 3
  - id: hook-length
    check: first_two_lines_char_count
    per_section: "^## LINKEDIN POST"
    max: 220
  - id: body-word-count
    check: section_word_count
    per_section: "^## LINKEDIN POST"
    min: 80
    max: 220
  - id: no-hashtag-spam
    check: hashtag_count
    max: 3
  - id: cta-present
    check: section_contains_any
    any_of: ["→", "Link:", "Read more", "Full post", "github.com/shopsys"]
```

Checker implementation: Claude reads files + YAML, reports pass/fail. No external binaries required — it's a literate check.

## Scheduling + measurement + auto-trigger (Loom talking points)

These are NOT built in phase 1 — only called out in PLAN.md and discussed in the Loom.

- **Scheduling:** after outputs exist, a follow-up skill `/schedule-drop <slug>` could push to Buffer/Publer API in an opinionated order: dev.to Mon → LinkedIn Tue → X thread Wed → Instagram Thu → Newsletter Fri → Shorts weekend. Each post tagged with UTM `utm_source=<channel>&utm_medium=organic&utm_campaign=<slug>`.
- **Measurement:** a `/perf-report` skill pulls GA4 + channel analytics by UTM, rolls up per-slug weekly. Identifies which platform × atom-type wins. Feeds back into `platforms/*.md` edits.
- **Auto-trigger:** RSS of shopsys blog → local launchd job (this is macOS; no cloud needed) → when new entry detected, spawn `claude -p "/blog-repurpose <url>" --allowedTools "WebFetch,Read,Write,Task"` in a dedicated directory. Output goes to `output/<slug>/` for human review before any posting.

## Test plan

- Target: https://www.shopsys.com/release-highlights-18-0-0/
- Pass criteria: 6 platform files in `output/release-highlights-18-0-0/`, `_atoms.json` populated, `_report.md` shows all rules pass (or only soft-fails).
- Spot-check: LinkedIn hook starts with a concrete stat or contrarian claim; Twitter thread is ≥5 tweets; Instagram carousel has 5 slides with the hook/stat/value/proof/CTA structure; Shorts script has 3s hook, beats, CTA; Dev.to has code-block-worthy technical depth.

## Build order

Phase 1 (build, parallelizable):
1. brand/shopsys.md
2. platforms/*.md (6 files)
3. conformance/*.yaml (6 files)
4. .claude/agents/*.md (8 files)
5. .claude/skills/blog-repurpose/SKILL.md

Phase 2 (dry-run test):
6. Simulate the skill on Release Highlights 18.0.0 — act as Claude would, produce all outputs, run checker, document findings in PLAN.md under "Post-run notes".

Phase 3 (docs):
7. USAGE.md for the user.

## Post-run notes

_(filled after the test run — record what the pipeline actually produced, any conformance gaps, and what I'd tune in the intent files to fix systemic issues.)_
