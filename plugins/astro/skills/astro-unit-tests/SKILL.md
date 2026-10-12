---
name: astro-unit-tests
description: Add or review focused unit and integration tests for Astro project logic with the repository's test runner, typically Vitest. Use for utilities, locale mapping, content transforms, and CMS adapters; use astro-responsive-qa for rendered layout.
---

# Astro unit and integration tests

Inspect the installed Astro version, test runner, scripts, aliases, and current tests before choosing an approach. Astro documents Vitest configuration via `getViteConfig()` and Playwright for browser flows; use the project's existing runner if one is present. Do not add a test solely to mirror a trivial implementation.

## Choose the boundary

- Unit-test pure transformations, locale and URL helpers, content mapping, validation, and error handling. Prefer inputs and observable outputs over private implementation details.
- For data fetching, control `fetch` or the adapter boundary with deterministic fixtures. Cover a successful response and meaningful failure behavior without real API credentials or network access.
- Treat rendered Astro markup, navigation, responsive layout, and hydration as browser behavior. Cover those with Playwright rather than pretending a utility test verifies the page.
- When no runner exists, propose the smallest setup that fits the target project's package manager and Astro version, then add it if the task authorizes implementation. Keep the test command explicit in `package.json`.
- Follow the adopted [Moduo project rules](../../rules/astro-project.md) in test code too: descriptive variable and parameter names with no single-letter names, short comments for fixture purpose or non-obvious setup, and reusable fixtures/helpers in small modules rather than repeated test scaffolding.

## Moduo landing cases

The reference project has locale helpers in `src/i18n/`, article query helpers in `src/lib/articles.ts`, local content schemas in `src/content/config.ts`, and a Strapi adapter in `src/lib/strapi.ts`. Useful tests would verify locale fallback, generated locale/query parameters, article mapping, and failure behavior. Do not copy the reference project's URLs, credentials, or fixtures into a new project.

Run the smallest relevant test command and the project's type or Astro check. Report the behavior covered, results, and gaps using the shared [QA report format](../../references/qa-report.md) when contributing to a combined release review. Read the [Astro testing guide](https://docs.astro.build/en/guides/testing/) for version-sensitive setup.
