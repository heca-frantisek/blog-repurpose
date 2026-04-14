# Release Highlights 18.0.0

**URL:** https://www.shopsys.com/release-highlights-18-0-0/
**Author:** Rostislav Vítek
**Date:** 2025-12-30
**Category:** Shopsys Platform

---

**Shopsys Platform 18.0.0 brings a comprehensive administration overhaul, powerful new promotional tools, enhanced security features, and significant developer experience improvements. This release focuses on modernizing the platform while adding essential e-commerce capabilities that help merchants drive sales and ensure regulatory compliance.**

## Feature Enhancements

### Administration Overhaul

The entire administration interface has been completely redesigned and modernized ([#3813](https://github.com/shopsys/shopsys/pull/3813)). The legacy custom-built theme has been replaced with the [Tabler UI](https://tabler.io/) framework, delivering a cleaner, more intuitive, and fully responsive interface across all devices.

**Key improvements:**

- Modern, responsive design that works seamlessly on desktop and mobile
- Improved form rendering, more standardized form type templates
- Reworked utility javascript for recommended length, copy to clipboard, dynamic placeholder
- Reimagined modal windows
- Order and complaint status colors for better visual identification
- Reduced codebase by approximately 12 000 lines of legacy code

### Product Gift

Merchants can now create gift campaigns where customers receive promotional products at special prices when purchasing qualifying items ([#4193](https://github.com/shopsys/shopsys/pull/4193)). Administrators can configure gift plans with specific validity periods, assign gift products to qualifying products, and set special gift pricing per domain.

The feature includes automatic flag management (products are automatically tagged as "Gift with Product"), real-time cart updates, and seamless integration with the checkout flow. Gift items appear in order summaries, confirmation emails, and order details.

### Promotion X + Y for Free

A powerful new promotional feature allows merchants to set up volume-based promotions directly on products ([#4194](https://github.com/shopsys/shopsys/pull/4194)). For example, a "3 + 1 free" promotion means that when a customer adds four items, they only pay for three — the fourth is free.

Products with active promotions are automatically flagged, making them easily identifiable to customers. The feature supports multi-domain configurations and processes flag updates asynchronously for optimal performance. This common retail strategy helps merchants drive volume sales and increase average order values.

### Order Withdrawal from Contract

This feature implements the Right of Withdrawal (cooling-off period) required by consumer protection laws in the European Union and many other jurisdictions. Customers can now submit withdrawal requests for eligible orders directly from their order detail page ([#4246](https://github.com/shopsys/shopsys/pull/4246)).

Administrators can configure the withdrawal deadline (typically 14 days after delivery) and customize withdrawal instructions per domain. A new "Withdrawn" order status has been added, and both customers and administrators receive email notifications when withdrawal requests are submitted.

### QR Payment for Bank Transfers

A new Bank Transfer payment method with QR code support has been added ([#4195](https://github.com/shopsys/shopsys/pull/4195)). When customers select bank transfer, they will see payment instructions with an embedded QR code containing all necessary payment details (IBAN, amount, variable symbol).

Administrators can configure bank account details (account number, IBAN, BIC/SWIFT) per domain and customize payment instructions using dynamic placeholders. QR codes are embedded directly in confirmation emails and order confirmation pages, making it easy for customers to complete payments using their banking app.

### Prevent Exceeding Available Stock

A new "Allow negative stock" option on products gives merchants control over whether customers can order more items than are currently in stock ([#4173](https://github.com/shopsys/shopsys/pull/4173)). When disabled, the system automatically adjusts cart quantities to match available stock and informs customers about the changes.

This prevents overselling for products where stock accuracy is critical, such as limited inventory, perishable goods, or exclusive items.

### Autocomplete Favorites

Administrators can now configure favorite products, brands, and categories that appear when users focus on the search autocomplete input before typing ([#4215](https://github.com/shopsys/shopsys/pull/4215)). This helps guide customers toward promoted or popular items.

The feature also improves search behavior for short queries (1–2 characters). While no results were previously shown, the search now returns basic matches using a simplified name-based search.

### Domain Configuration with Path Fragment

Multi-domain setups can now use path-based URL structures instead of subdomains, for example example.com/cz and example.com/sk. This reduces the need to manage multiple hostnames and allows all locales to run under a single domain. From an SEO perspective, this can simplify setup and signal a unified site structure. The feature is particularly useful for multi-locale deployments or regional variants ([#4113](https://github.com/shopsys/shopsys/pull/4113)).

## Design & Appearance

The storefront saw a range of visual, UX, and accessibility improvements focused on clarity and smoother interactions.

Order lists now display product thumbnails, making purchased items easier to recognize at a glance ([#4213](https://github.com/shopsys/shopsys/pull/4213)).

Content editors can create richer and more consistent storefront content thanks to new WYSIWYG text styles available in CKEditor ([#4208](https://github.com/shopsys/shopsys/pull/4208)).

The contact information section was redesigned with a clearer layout and improved structure ([#4221](https://github.com/shopsys/shopsys/pull/4221)), while the reset password page received a cleaner, more modern visual update ([#4254](https://github.com/shopsys/shopsys/pull/4254)).

Search and navigation were refined with improved autocomplete behavior and UX, including better accessibility and a more polished popup design ([#4215](https://github.com/shopsys/shopsys/pull/4215), [#4230](https://github.com/shopsys/shopsys/pull/4230)). Accessibility was further enhanced through improvements to the banner slider and better visual feedback during cart operations, such as corrected loading overlay positioning ([#4158](https://github.com/shopsys/shopsys/pull/4158), [#4155](https://github.com/shopsys/shopsys/pull/4155)).

Scrolling behavior on customer order and complaint pages was refined for smoother navigation ([#4189](https://github.com/shopsys/shopsys/pull/4189)), the product image gallery opening was unified to prevent visual stacking issues ([#4273](https://github.com/shopsys/shopsys/pull/4273)), and several minor storefront style fixes and performance optimizations improved overall visual stability and perceived speed ([#4253](https://github.com/shopsys/shopsys/pull/4253), [#3953](https://github.com/shopsys/shopsys/pull/3953)).

## Developer Experience

### Symfony Clock Integration

Direct usage of DateTime and DateTimeImmutable has been replaced with Symfony Clock across the codebase, establishing a consistent approach to time handling ([#4297](https://github.com/shopsys/shopsys/pull/4297)).

This change enables deterministic testing by allowing full control over what "now" means, eliminates flaky tests caused by timing issues, and provides clear guidelines for working with time.

### DataSource Factories with Collation Support

All Grid DataSource implementations now use the factory pattern, improving extensibility and dependency injection. More importantly, QueryBuilderDataSources now automatically apply locale-specific collation to textual columns, ensuring grid data is sorted correctly according to the administrator's language settings. Czech characters (č, ř, š, ž) now sort correctly when a Czech administrator is logged in ([#4135](https://github.com/shopsys/shopsys/pull/4135)).

### Asynchronous Order Email Preparation

Email handling for new orders is now fully asynchronous ([#4266](https://github.com/shopsys/shopsys/pull/4266)). Previously, only the sending itself was deferred, while email preparation (loading templates and resolving variables) still ran synchronously during order placement and could slow down checkout. The preparation step has now been moved out of the critical path, resulting in faster order creation and a lower risk of checkout failures.

### Reduced Product Detail Data Transfer

GraphQL queries for product detail pages now return only necessary store availability data instead of full store information. This can reduce payload size by 70-80% for store availability data, resulting in faster API responses and improved (not only) mobile performance ([#4324](https://github.com/shopsys/shopsys/pull/4324)).

## Conclusion

The updates highlighted above showcase some of the most significant changes and innovations in the latest release of the Shopsys Platform. For a complete list of improvements, visit the [release page](https://github.com/shopsys/shopsys/releases/tag/v18.0.0) on our GitHub. You can explore a comprehensive overview of the platform on the [Shopsys website](https://www.shopsys.com/shopsys-platform/), and dive deeper through our [knowledge base](https://docs.shopsys.com/en/18.0/). If you have any questions, suggestions, or contributions, we welcome you to join the conversation on [GitHub Discussions](https://github.com/shopsys/shopsys/discussions), [submit an issue](https://github.com/shopsys/shopsys/issues/new/choose), or [open a pull request](https://github.com/shopsys/shopsys/compare). Your feedback plays a crucial role in shaping the future of the platform.
