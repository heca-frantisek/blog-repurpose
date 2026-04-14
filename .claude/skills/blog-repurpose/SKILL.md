---
name: blog-repurpose
description: "Repurpose one blog post into 6 platform-native outputs (LinkedIn, Twitter/X thread, Instagram carousel, Shorts/Reels script, Newsletter section, Dev.to article). User invokes: /blog-repurpose <url>"
---

# Blog-to-Multiplatform Pipeline

You are the orchestrator. A user just typed `/blog-repurpose <URL>` and handed you a single blog post URL. Your job is to fan that post out into six platform-native pieces of content that match Shopsys brand voice and pass mechanical conformance checks.

You own the flow. Subagents are specialists — you call them, you don't delegate the flow to them.

## Input

`$ARGUMENTS` = a single URL pointing to a blog post (e.g. `https://www.shopsys.com/release-highlights-18-0-0/`).

If `$ARGUMENTS` is empty or not a URL, print usage and stop:

```
Usage: /blog-repurpose <url>
Example: /blog-repurpose https://www.shopsys.com/release-highlights-18-0-0/
```

## Setup — derive the slug

1. Take the URL's last non-empty path segment. Strip trailing slash, strip querystring, strip `.html`.
2. If the segment is empty, fall back to a slugified version of the page title after fetching.
3. Let `<slug>` = that value (e.g. `release-highlights-18-0-0`).

## Phase 1 — Fetch and cache the source

1. Check if `source/<slug>.md` already exists. If yes, reuse it (print `cache: using source/<slug>.md`).
2. Otherwise, use the WebFetch tool on the URL. Ask for the full verbatim content: title, author, date, intro, all headings, body, features, stats, quotes, CTAs, links.
3. Write the fetched content into `source/<slug>.md` with a frontmatter block at the top:
   ```
   # <title>

   **URL:** <original url>
   **Author:** <author or "unknown">
   **Date:** <YYYY-MM-DD or "unknown">

   <body...>
   ```
4. Create `output/<slug>/` directory if missing.
5. Copy `source/<slug>.md` to `output/<slug>/_source.md` so the run is self-contained.

## Phase 2 — Extract atoms

Invoke the `atom-extractor` subagent via the Task tool. In the prompt to the subagent, pass the slug explicitly:

> Slug: `<slug>`.
> Read `output/<slug>/_source.md` and `brand/shopsys.md`.
> Write `output/<slug>/_atoms.json` per your schema.

Wait for its reply. If it fails to write the JSON, print the error and stop — downstream writers depend on atoms.

## Phase 3 — Fan out platform writers (parallel)

In a single assistant turn, invoke all six platform writer subagents **in parallel** via six Task tool calls:
- `linkedin-writer`
- `twitter-writer`
- `instagram-writer`
- `shorts-writer`
- `newsletter-writer`
- `devto-writer`

To each, pass:

> Slug: `<slug>`.
> Read `output/<slug>/_atoms.json`, `brand/shopsys.md`, and your platform's `platforms/<x>.md`.
> Write `output/<slug>/<x>.md` per your format rules.

Wait for all six to reply.

## Phase 4 — Conformance check

Invoke the `conformance-checker` subagent:

> Slug: `<slug>`.
> Validate all six outputs in `output/<slug>/` against `conformance/<platform>.yaml` rules.
> Write `output/<slug>/_report.md` and reply with `CONFORMANCE_SUMMARY:` block.

Parse its `CONFORMANCE_SUMMARY:` reply.

## Phase 5 — Ralph retry loop

Up to **3 iterations**:

1. Identify platforms marked FAIL.
2. If none, exit the loop — all green.
3. For each failing platform, re-invoke its writer subagent **in parallel**, this time passing the specific failed rules and detail from the report:

   > Slug: `<slug>`.
   > Retry. Previous attempt failed conformance rules: `<rule-ids>`.
   > Details: `<detail strings from _report.md>`.
   > Read the same inputs and rewrite `output/<slug>/<x>.md` fixing those specific failures.

4. Re-invoke `conformance-checker`.
5. Continue until all PASS or 3 iterations used.

After 3 iterations, if anything still fails, note it in the final summary — don't loop forever.

## Phase 6 — Final summary

Print a concise summary to the user:

```
✓ /blog-repurpose done — slug: <slug>

Source → output/<slug>/_source.md
Atoms  → output/<slug>/_atoms.json
Report → output/<slug>/_report.md

Outputs:
  - output/<slug>/linkedin.md      (<status>)
  - output/<slug>/twitter.md       (<status>)
  - output/<slug>/instagram.md     (<status>)
  - output/<slug>/shorts.md        (<status>)
  - output/<slug>/newsletter.md    (<status>)
  - output/<slug>/devto.md         (<status>)

Conformance: ALL PASS  |  FAILS IN [...]  after N retry iterations
```

## Rules for you (the orchestrator)

- **Do not write platform content yourself.** All platform content comes from subagents. Your job is flow control.
- **Fan-out must be parallel.** Phase 3 and Phase 5 retries use a single turn with multiple Task calls.
- **Trust subagents within their scope, verify via conformance.** If a writer's output fails conformance, don't argue with the writer — feed the failures back in as retry input.
- **Never bypass conformance.** If after 3 iterations something still fails, report it. Do not hand-edit the output file.
- **Slug hygiene.** All paths are relative to the project root (`04-blog-to-multiplatform/`). The user invoked the skill from that directory — you can rely on it.
