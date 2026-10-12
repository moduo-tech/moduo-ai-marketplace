---
name: astro-cms
description: Build or review Astro content and CMS adapters with typed data, explicit failure behavior, publication rules, and deterministic tests. Use for Strapi or other CMS integrations, blog builds, and content pipelines.
---

# Astro CMS and content pipeline

Inspect the project's CMS, content collections, types, build mode, environment variables, adapters, and publication rules before changing data access. Follow the adopted [Moduo project rules](../../rules/astro-project.md): keep fetching in services or adapters, transformations in reusable helpers, and page templates focused on presentation.

## Define the boundary

- Keep credentials on the server or at build time. Validate required configuration and response shape. Distinguish empty content, missing content, invalid data, authorization errors, timeouts, and service failures; do not cast failures to a successful empty list unless the product explicitly defines that behavior.
- Map remote records into stable application types before rendering. Normalize URLs and media safely, validate locale and publication status, and make missing optional fields explicit in the UI.
- For static builds, decide how a CMS outage affects the build and publication. Test list, detail, pagination, RSS and sitemap outputs when those features exist. Avoid shipping stale or incomplete output silently when the content is required.
- Use deterministic fixtures to test transformations and failure paths without real credentials or network requests. Check generated pages as well as adapter tests when routing or SEO depends on CMS data.
- In `moduo-landing`, inspect `src/lib/strapi.ts`, `src/lib/articles.ts`, article interfaces, blog pages, RSS, and local content schema. The adapter currently returns an empty array on HTTP and fetch errors; treat that as behavior to evaluate against the product requirement, not a pattern to copy.

Report the content contract, failure policy, tests run, generated pages checked, and unresolved data dependencies. Use `astro-unit-tests` for focused adapter coverage and `astro-seo` for indexability or metadata effects.

References: [Astro content collections](https://docs.astro.build/en/guides/content-collections/), [content loader API](https://docs.astro.build/en/reference/content-loader-reference/), and [Astro with Strapi](https://docs.astro.build/en/guides/cms/strapi/).
