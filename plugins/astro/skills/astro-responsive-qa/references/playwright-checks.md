# Playwright checks for Astro layouts

Adapt this example to the project's routes, locale, breakpoints, and existing Playwright configuration. It checks one observable invariant without making screenshot pixels the only proof of a usable layout.

```ts
import { test, expect } from '@playwright/test';

for (const width of [360, 390, 768, 1024, 1440]) {
  test(`home remains usable at ${width}px`, async ({ page }) => {
    await page.setViewportSize({ width, height: 900 });
    await page.goto('/');

    await expect(page.getByRole('heading', { level: 1 })).toBeVisible();
    const overflow = await page.evaluate(
      () => document.documentElement.scrollWidth > window.innerWidth + 1
    );
    expect(overflow, 'document must not overflow horizontally').toBe(false);

    // Assert the actual menu or CTA behavior relevant to this route here.
  });
}
```

Add widths just below and above affected custom breakpoints when a layout changes there. A passing overflow assertion does not rule out overlapping or clipped elements; inspect screenshots and exercise controls. For screenshot comparisons, wait for the final page state, use stable fixture data, and review baseline changes manually. Keep visual baselines on the same browser and operating system used in CI.
