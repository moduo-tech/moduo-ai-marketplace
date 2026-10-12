---
name: astro-functional-qa
description: Review an Astro site or web app as a user in a real browser, checking navigation, links, menus, forms, and key journeys against the site's apparent purpose. Use for functional QA before release or after interaction changes; produce a reproducible report.
---

# Functional product QA

Inspect the project, its routes, navigation, content, and stated product goals before testing. Identify the main user journeys and what each visible control appears to promise. Use the running site in a real browser, preferably the project's Playwright setup, to verify those expectations. Treat ambiguous destinations as UX findings, not confirmed defects, until evidence establishes the intended behavior.

## Explore as a user

1. Map the relevant routes, locales, navigation groups, calls to action, forms, and interactive components. Prioritize the landing journey, primary conversion flow, and recently changed areas. Record the tested environment and route scope.
2. Follow links and controls from the rendered page. Confirm internal links resolve to the expected content, anchors land on existing targets, locale links reach their corresponding page, and menus open, close, and navigate by mouse and keyboard. Check that the logo, breadcrumb, back links, cards, buttons, tabs, accordions, dialogs, and mobile navigation behave as their labels imply where present.
3. Exercise form validation, success and error feedback, disabled or loading states, and empty or not-found states when they are available. Use safe test data in a controlled environment; do not send real submissions, purchases, messages, or other consequential production actions without authorization.
4. Check external links for valid targets where feasible. Distinguish an actual broken destination from an unavailable network, authentication wall, bot protection, or third-party failure. Inspect console errors and failed requests that affect the journey, and reproduce suspicious behavior before reporting it.
5. Use role-based locators and web-first assertions for repeatable Playwright checks. Do not claim a route or control works from source inspection alone. When writing test code, follow the adopted [Moduo project rules](../../rules/astro-project.md), including descriptive names, brief explanatory comments, and reusable helpers.

## Report

Use the shared [QA report format](../../references/qa-report.md). For each finding, include the user action, expected result based on the page's purpose, observed result, route, environment, evidence, severity, and reproduction steps. Separate confirmed defects, UX ambiguities, and checks blocked by the environment. Record tested journeys and meaningful gaps even when no defects are found. Do not mark production readiness as passed if core journeys were not exercised.

For layout and viewport coverage, use `astro-responsive-qa`; for search visibility, use `astro-seo`. Their reports feed the same final QA summary.

Reference: [Playwright best practices](https://playwright.dev/docs/best-practices) and [locators](https://playwright.dev/docs/locators).
