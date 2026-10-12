---
name: astro-i18n-content
description: Implement or audit localized Astro routes and content for completeness, correct destinations, language metadata, and consistent user journeys. Use for multilingual page or content changes.
---

# Localized routes and content

Inspect `astro.config.*`, configured locales, route files, translation sources, content collections or CMS locales, locale helpers, and link generation before editing. Preserve the project's routing strategy; do not assume every Astro project uses automatic i18n routing.

## Check the locale journey

- Map corresponding routes for each configured locale. Verify navigation, calls to action, forms, 404 pages, pagination, blog links, and language switchers point to meaningful localized destinations. Record pages without an equivalent rather than inventing a translation.
- Check that visible copy, metadata, `html lang`, canonical and alternate links, images with localized text, and structured content match the selected locale. Coordinate search metadata findings with `astro-seo` rather than reporting the same root problem twice.
- Test direct navigation and browser navigation, including redirects and client-side transitions when used. Verify locale preference or cookies do not trap users, create loops, or redirect to a missing route.
- For content, validate required fields and locale relationships before publication. Use a focused test for locale URL helpers and content mapping when behavior changes.
- In `moduo-landing`, inspect root Spanish and `/en` routes, `src/i18n/`, `src/lib/articles.ts`, language preference logic in `Layout.astro`, and localized Strapi blog pages. Its current approach is a reference, not the default for other projects.

For a review, use the shared [QA report format](../../references/qa-report.md) with `I18N` finding IDs, a route-pair matrix, observed behavior, evidence, and missing or blocked content. Send the report to the QA coordinator for a combined release review.

Reference: [Astro i18n routing](https://docs.astro.build/en/guides/internationalization/) and [Astro i18n API](https://docs.astro.build/en/reference/modules/astro-i18n/).
