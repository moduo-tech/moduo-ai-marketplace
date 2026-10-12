---
name: astro-accessibility
description: Implement or review accessibility in Astro pages and components, including keyboard use, focus, semantics, forms, contrast, motion, and browser checks. Use for accessibility work or a release audit.
---

# Astro accessibility

Inspect the project's components, routes, interactive behavior, and current accessibility tooling. Follow the adopted [Moduo project rules](../../rules/astro-project.md) when changing code. Test the rendered UI in a browser; source inspection and automated scans alone cannot establish accessibility.

## Check and improve

- Use semantic landmarks, headings, links, and buttons. Give controls an accessible name and meaningful state. Keep focus visible and in a sensible order through menus, dialogs, forms, and route changes.
- Navigate important journeys using only the keyboard. Open and close menus, submit forms, inspect error messages, and verify focus after navigation. When a project uses Astro's `ClientRouter`, check route titles, announcements, focus, and repeated script initialization.
- Check image alternatives, field labels, validation feedback, contrast in actual component states, zoom and narrow viewports, and reduced-motion behavior. Review both default and interactive states.
- Use Playwright with `@axe-core/playwright` when available to catch automatically detectable issues. Inspect the resulting elements and severity; follow with manual keyboard and visual checks. Do not claim an automated scan covers every accessibility requirement.
- For `moduo-landing`, inspect the Spanish and English header/menu, contact forms, cookie dialog, blog, and pages using `ClientRouter`. These are review targets, not assumptions that defects exist in other Astro projects.

For an audit, use the shared [QA report format](../../references/qa-report.md) with `A11Y` finding IDs, routes and states checked, steps, evidence, impact, and gaps. Send the specialist report to the QA coordinator for a combined release review.

References: [Playwright accessibility testing](https://playwright.dev/docs/accessibility-testing), [W3C WCAG quick reference](https://www.w3.org/WAI/WCAG22/quickref/), and [Astro client routing](https://docs.astro.build/en/guides/view-transitions/).
