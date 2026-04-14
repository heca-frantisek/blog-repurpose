# LinkedIn — release-highlights-18-0-0

## LINKEDIN POST 1 — Migration story (admin overhaul)

We deleted ~12,000 lines of admin code in Shopsys 18.0.0.

The admin still has every feature. It just runs on Tabler UI instead of a decade of bespoke theming.

What that legacy cost us:
→ Six different form template conventions across screens
→ Modals that fought every browser update
→ A mobile admin nobody actually wanted to use
→ Onboarding ramp measured in weeks, not days

What replaced it:
→ One framework, standardized form templates, reworked modals and utility JS
→ Responsive by default — the admin works on a phone now
→ Shared components instead of per-screen snowflakes

The honest part: this isn't a new feature. It's a cleanup. But "legacy" isn't what you built — it's what nobody wants to touch. Cutting 12k lines of that is the feature.

We shipped it alongside gift-with-purchase, X+Y volume promos, QR bank transfers, and EU order withdrawal. The admin overhaul is the one that pays off every day for the next five years.

Full release notes → https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=linkedin&utm_medium=organic&utm_campaign=release-highlights-18-0-0

#ecommerce #symfony #opensource

## LINKEDIN POST 2 — Behind-the-scenes (async email)

Checkout was slow because we lied to ourselves about what "async email" meant.

Only the sending was deferred. Template loading and variable resolution still ran in the checkout critical path.

So every order paid for:
→ Fetching the email template
→ Resolving every placeholder and variable
→ Rendering the body
→ …and only then handing it to the queue

On a bad day, that added latency and a nonzero chance of checkout failure if template resolution threw. On a good day, it was invisible tech debt quietly taxing conversion.

In 18.0.0 (PR 4266), the entire preparation step is out of the critical path. Order creation finishes first. The email gets built and sent by a worker afterward.

Same user-visible behavior. Faster checkout. Fewer failure modes coupled to a template bug.

The lesson we keep relearning: "async" is a claim you have to verify at the boundary. If anything synchronous happens before the enqueue, you didn't move it off the critical path — you just renamed it.

Full changelog → https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=linkedin&utm_medium=organic&utm_campaign=release-highlights-18-0-0

#symfony #devex #ecommerce

## LINKEDIN POST 3 — Merchant-impact (compliance + payments)

EU cooling-off rights have been legally required for years. Most Shopsys projects shipped them as a custom patch.

In 18.0.0, order withdrawal is a first-class feature.

What merchants get without custom dev:
→ Configurable withdrawal deadline (typically 14 days after delivery)
→ Per-domain withdrawal instructions for multi-market stores
→ A dedicated "Withdrawn" order status in the admin
→ Email notifications to both customer and operator
→ Customers trigger it themselves from their account — no support ticket

Shipped alongside it, in the same release:
→ QR Payment for Bank Transfers — IBAN, amount, and variable symbol embedded in the confirmation email, scannable from any Czech or EU banking app
→ Product Gift and X+Y volume promotions, configurable per product and per domain
→ Prevent Exceeding Available Stock, so overselling stops being a manual cleanup job

The pattern: compliance and merchandising features that used to live in the "custom work" column are now in the platform. Less bespoke code per project, less drift between deployments.

Full release notes → https://github.com/shopsys/shopsys/releases/tag/v18.0.0?utm_source=linkedin&utm_medium=organic&utm_campaign=release-highlights-18-0-0

#ecommerce #opensource
