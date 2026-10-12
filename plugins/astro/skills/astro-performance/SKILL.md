---
name: astro-performance
description: Measure and improve the loading and interaction performance of Astro pages, including images, fonts, scripts, hydration, and layout stability. Use for performance work or release audits with real measurements.
---

# Astro performance

Inspect the project's build mode, image pipeline, fonts, scripts, client directives, and existing measurement commands. Measure representative built pages in a repeatable browser environment before proposing fixes. Keep lab results separate from field data and avoid invented performance scores.

## Measure and improve

- Cover the homepage, a content-heavy page, and a key conversion page; include relevant locales and mobile/desktop conditions. Record URL, build, browser, viewport, network/device settings, tool, and run conditions.
- Use the project's existing Lighthouse or Unlighthouse workflow when present. Inspect LCP, CLS, and a lab responsiveness proxy such as TBT; use field data for INP when available. Recheck after a change using comparable conditions. If no performance budget exists, report measurements and trends without inventing a pass threshold.
- Identify the actual LCP element and layout shifts before editing. Check image dimensions, responsive sources, formats, loading priority, font loading, third-party scripts, and unnecessary client JavaScript or hydration.
- Prefer Astro's image handling and static rendering where they fit the project's setup. Preserve visual fidelity, accessibility, and required interactions; do not remove functionality to improve a score.
- In `moduo-landing`, inspect the existing `unlighthouse` script, `src/components/common/Image.astro`, image optimization helpers, `ClientRouter`, fonts, consent and analytics scripts, and Strapi-backed blog images. These are reference areas, not a universal stack.

For an audit, use the shared [QA report format](../../references/qa-report.md) with `PERF` finding IDs, baseline measurements, evidence, likely cause, proposed fix, and verification result or gap. Send the report to the QA coordinator for a combined release review.

References: [Astro image guide](https://docs.astro.build/en/guides/images/), [Astro islands](https://docs.astro.build/en/concepts/islands/), and [Web Vitals measurement](https://web.dev/articles/vitals-measurement-getting-started).
