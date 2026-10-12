---
name: astro-responsive-qa
description: Validate an Astro page's responsive layout and interactions in a real browser with Playwright. Use after visible UI changes or when asked for mobile, tablet, desktop, screenshot, or visual regression review.
---

# Responsive browser validation

Use the target project's Playwright installation or available browser tooling. If it is absent and implementation is in scope, add the smallest Playwright setup that the project needs. Start the site with its documented command; static builds that depend on a CMS need stable local data or a controlled test environment. Never put credentials in tests or screenshots.

When writing browser tests for a project that has adopted the [Moduo project rules](../../rules/astro-project.md), use descriptive variable and parameter names with no single-letter names. Keep viewport data, reusable setup, and page interactions in small helpers; add short comments where test intent or setup is not obvious.

## Inspect the page

1. Identify affected routes, locales, interactions, and project breakpoints from the actual configuration. Include a narrow mobile width, a wider phone, tablet, desktop, and widths just below and above any breakpoint that changes the affected layout. For `moduo-landing`, inspect around its custom 475, 768, 890, 1024, and 1280 pixel breakpoints when relevant; these are examples, not universal defaults.
2. At each selected width, inspect the rendered page and test for horizontal document overflow, clipped or overlapping content, readable wrapping, navigation and menu behavior, focus visibility, and controls that remain usable by touch and keyboard. Test both configured locales where shared UI is affected.
3. Use role-based locators and web-first assertions for behavior. Avoid fixed sleeps. Capture screenshots only after fonts, images, and dynamic content are stable. For visual regression, keep baselines on a consistent browser and OS, mask truly variable regions, and review intentional baseline updates.
4. Inspect the actual screenshots, not only the exit code. Report route, viewport, issue location, evidence, and whether the failure is functional, visual, or environmental.

If the browser or site cannot run, state the exact blocker and retain the static checks already completed. Do not report responsive validation as passed from source inspection alone.

Produce a specialist report using the shared [QA report format](../../references/qa-report.md), including the viewports and routes covered, reproducible findings, working behavior, and blocked or untested checks. Pass the report to the QA coordinator when a combined release review is requested.

For a reusable automated example, read [Playwright checks](references/playwright-checks.md) only when creating or revising browser tests. Adapt its routes, widths, and locators to the actual project.

Reference: [Playwright best practices](https://playwright.dev/docs/best-practices), [emulation](https://playwright.dev/docs/emulation), and [visual comparisons](https://playwright.dev/docs/test-snapshots).
