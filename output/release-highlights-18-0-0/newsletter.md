# Newsletter — release-highlights-18-0-0

## SUBJECT LINE
Shopsys 18.0: -12k admin lines, -70% payload

## PREVIEW TEXT
Tabler UI admin, async checkout email, EU withdrawal, QR transfers, gift promos.

## SECTION BODY
Shopsys 18.0.0 ships with roughly 12,000 fewer lines of legacy admin code and a 70-80% smaller payload on product detail GraphQL store availability.

The headline is a full admin overhaul on [Tabler UI](https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=newsletter&utm_medium=email&utm_campaign=release-highlights-18-0-0) replacing the custom legacy theme — responsive, lighter to maintain, and standardized across modals and form templates. Alongside it, merchants get the promotional and compliance toolbox they usually had to build themselves, and checkout shed a long-standing synchronous step.

- **Tabler UI admin** — modern responsive administration, ~12,000 lines of bespoke theme code deleted.
- **Order Withdrawal from Contract** — EU cooling-off compliance out of the box, configurable 14-day deadline, dedicated "Withdrawn" status, per-domain instructions.
- **Product Gift and X+Y for Free** — gift-with-purchase plans and volume promos like "3+1" set per product, with per-domain pricing and email integration.
- **QR Payment for Bank Transfers** — embedded QR codes with IBAN, amount, and variable symbol in confirmation emails and order pages.
- **Async order email preparation** — template rendering and variable resolution moved out of the checkout critical path, cutting order creation time and failure risk.

Also in this release: Symfony Clock everywhere, locale-aware Grid DataSource collation (Czech diacritics sort correctly), path-fragment multi-domain URLs, and strict stock control to kill overselling.

Full release → [release notes](https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=newsletter&utm_medium=email&utm_campaign=release-highlights-18-0-0)
