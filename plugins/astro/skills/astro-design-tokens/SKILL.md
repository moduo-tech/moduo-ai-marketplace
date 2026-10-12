---
name: astro-design-tokens
description: Translate an existing Astro project's visual language or a Penpot design into reusable Tailwind theme tokens and component patterns. Use before new visual implementation, redesigns, or theme consolidation.
---

# Design to Tailwind theme

Follow the adopted [Moduo project rules](../../rules/astro-project.md). Inspect the installed Tailwind version, current config or CSS theme, global styles, components, and project references. Preserve the existing visual language unless the user requests a change.

## Establish the source

1. Look for a relevant design even if the user did not explicitly mention one. If it is in Penpot, use the Penpot MCP to inspect its pages, components, styles, assets, variants, and measurements. Do not infer values that are available in the design. If access is missing, request a usable design reference.
2. If there is no design, summarize the project's current palette, typography, spacing, radii, shadows, breakpoints, and patterns. Ask the user whether to preserve or evolve them before laying out new UI. For a new project, ask for visual references and direction before defining the theme.
3. Map reusable values to semantic tokens in `tailwind.config.*` for Tailwind 3 or CSS `@theme` for Tailwind 4, according to the installed version. Document the relationship between design values and tokens; prefer names by role over raw color names where the project supports them.
4. Build or adjust a small set of representative components, then compare rendered mobile and desktop states with the design. Resolve repeated arbitrary values by adding tokens. Keep literal hex values in the theme definition rather than scattering them through `.astro` templates or stylesheets.

In `moduo-landing`, inspect `tailwind.config.js`, `src/assets/styles/tailwind.css`, `src/components/v2/`, and the custom breakpoints and `mo-*` palette. Use those as evidence of an existing system; do not transplant its tokens into unrelated projects.

Hand implementation to `astro-ui` after the theme and component patterns are established. Report the source design, token changes, component examples, visual checks, and any decisions awaiting the user.

Reference: [Tailwind theme variables](https://tailwindcss.com/docs/theme/) and [responsive design](https://tailwindcss.com/docs/responsive-design).
