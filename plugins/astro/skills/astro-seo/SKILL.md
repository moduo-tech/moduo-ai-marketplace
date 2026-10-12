---
name: astro-seo
description: Implement or audit technical SEO in an Astro site, including rendered metadata, canonical URLs, localized routes, sitemap, structured data, and indexability. Use for SEO requests or changes to pages and content that affect search visibility.
---

# Astro technical SEO

Inspect `astro.config.*`, the site's base URL and route format, layouts, metadata helpers, robots and sitemap configuration, content source, and all configured locales. Check the generated HTML or running page; source templates alone do not prove the final tags and links.

When implementing a fix in a project that has adopted the [Moduo project rules](../../rules/astro-project.md), keep metadata markup in focused components and move reusable URL or locale logic into small utilities. Use descriptive names and brief comments for non-obvious rules.

## Audit or implement

- Confirm each indexable page has a descriptive title and description, one intended canonical URL with the project's trailing-slash policy, and the expected robots directives. Do not canonicalize distinct language pages to one language.
- For localized equivalents, verify `<html lang>`, reciprocal `hreflang` links when the site uses them, language switcher destinations, and that each locale's text and metadata actually match its route. Do not invent a translation or an alternate URL that does not exist.
- Check that sitemap entries represent the intended indexable routes and exclude error pages. Check robots configuration without assuming robots.txt can enforce deindexing.
- Validate JSON-LD types and values against visible page content, especially article title, URL, image, author, and dates. Do not add structured data merely to chase rich results.
- Check meaningful image alternatives, link text, status codes, internal links, and page rendering for content loaded from a CMS. Distinguish a missing article from an API failure; a silent empty response can hide SEO defects.
- Measure or report performance signals separately from indexability. Avoid invented rankings, traffic gains, or guaranteed search outcomes.

In `moduo-landing`, inspect the shared `Metadata.astro` and `StructuredData.astro` components, Spanish root and `/en` routes, blog pages generated from Strapi, and sitemap integration. The repository's implementation is evidence to audit, not a mandatory template for every Astro project.

Return a specialist report using the shared [QA report format](../../references/qa-report.md): prioritized findings with affected URLs or files, observed output, why it matters, a concrete fix, routes checked, and any blocked checks. Pass it to the QA coordinator when a combined release review is requested. Cite [Google Search Central](https://developers.google.com/search/docs/fundamentals/seo-starter-guide), its [localized-pages guidance](https://developers.google.com/search/docs/specialty/international/localized-versions), and Astro documentation for version-sensitive behavior.
