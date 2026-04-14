# Conformance report — release-highlights-18-0-0

## Summary
- linkedin: PASS (0 hard fail, 0 soft fail)
- twitter: PASS (0 hard fail, 0 soft fail)
- instagram: PASS (0 hard fail, 0 soft fail)
- shorts: PASS (0 hard fail, 0 soft fail)
- newsletter: PASS (0 hard fail, 0 soft fail)
- devto: PASS (0 hard fail, 0 soft fail)

Overall: ALL PASS

## linkedin
| Rule | Severity | Status | Detail |
|---|---|---|---|
| post-count | hard | PASS | 3 sections matching `^## LINKEDIN POST` (min 2, max 3) |
| hook-length | hard | PASS | first-two-lines combined: post1=58, post2=78, post3=113 chars (max 220) |
| body-word-count | hard | PASS | post word counts ~190 / ~200 / ~205 (range 80-220) |
| hashtag-limit | hard | PASS | hashtags per post: 3 / 3 / 2 (max 3); no stray `#` tokens in bodies |
| cta-link-present | hard | PASS | utm_source=linkedin present in all 3 posts |
| no-banned-openers | soft | PASS | hooks start with "We deleted", "Checkout was slow", "EU cooling-off" — none forbidden |
| no-emoji-spam | soft | PASS | only arrow glyphs (→) used, not counted as emoji |

## twitter
| Rule | Severity | Status | Detail |
|---|---|---|---|
| tweet-count | hard | PASS | 9 tweet lines matching `^\d+/` (range 5-9) |
| per-tweet-length | hard | PASS | longest tweet = tweet 9 at ~156 chars (max 280) |
| cta-link-present | hard | PASS | final tweet contains utm_source=twitter |
| hashtag-discipline | soft | PASS | 0 hashtags across thread (max 1) |
| one-link-only | hard | PASS | exactly 1 URL in thread |
| no-banned-openers | soft | PASS | tweet 1 begins with "1/ Shopsys 18.0.0 shipped." |

## instagram
| Rule | Severity | Status | Detail |
|---|---|---|---|
| slide-count | hard | PASS | 5 slides (exact 5) |
| required-slots | hard | PASS | slide headings match expected order (Hook, Stat/Claim, Value, Proof, CTA) |
| slide1-word-limit | hard | PASS | 10 words incl. [visual:] line (max 12) |
| slide2-word-limit | hard | PASS | 13 words (max 18) |
| slide3-word-limit | hard | PASS | 21 words (max 30) |
| slide4-word-limit | hard | PASS | 24 words (max 30) |
| slide5-word-limit | hard | PASS | 10 words (max 15) |
| caption-present | hard | PASS | ## CAPTION section contains utm_source=instagram |
| caption-length | soft | PASS | caption body ~294 chars (max 300) |
| visual-direction-present | soft | PASS | all 5 slides contain `[visual:` note |

## shorts
| Rule | Severity | Status | Detail |
|---|---|---|---|
| required-sections | hard | PASS | HOOK, BEAT 1-3, CTA, SHOT LIST, ON-SCREEN TEXT, CAPTION all present |
| hook-word-limit | hard | PASS | 16 words (max 18) |
| beats-word-limit | soft | PASS | beats ~39 / ~38 / ~42 words (range 15-55) |
| caption-length | hard | PASS | CAPTION 191 chars (max 200) |
| cta-present | hard | PASS | CTA contains "link in bio" and "github.com/shopsys" |
| shot-list-bullets | soft | PASS | 5 bullets in SHOT LIST (min 3) |

## newsletter
| Rule | Severity | Status | Detail |
|---|---|---|---|
| required-sections | hard | PASS | SUBJECT LINE, PREVIEW TEXT, SECTION BODY all present |
| subject-length | hard | PASS | 44 chars (max 50) |
| preview-length | hard | PASS | 80 chars (max 90) |
| subject-no-emoji | hard | PASS | 0 emoji in subject |
| body-word-count | hard | PASS | ~215 words (range 180-260) |
| bullets-present | hard | PASS | 5 bullets (range 3-5) |
| cta-link | hard | PASS | SECTION BODY contains utm_source=newsletter |
| no-greeting | soft | PASS | no forbidden greetings found |

## devto
| Rule | Severity | Status | Detail |
|---|---|---|---|
| required-sections | hard | PASS | TITLE, TAGS, COVER BLURB, ARTICLE BODY all present |
| title-length | hard | PASS | 61 chars (max 70) |
| tags-count | hard | PASS | 4 comma-separated tags (range 3-4) |
| blurb-length | hard | PASS | 160 chars (max 180) |
| article-body-word-count | hard | PASS | ~760 words (range 500-900) |
| code-block-present | hard | PASS | fenced ```php and ```yaml blocks present |
| headings-present | soft | PASS | 5 H3 sub-headings (min 2) |
| cta-link | hard | PASS | ARTICLE BODY contains utm_source=devto |
