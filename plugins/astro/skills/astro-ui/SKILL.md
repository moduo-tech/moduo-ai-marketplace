---
name: astro-ui
description: Build or refine Astro pages and components from the project's visual theme or a Penpot design, with Tailwind-first, mobile-first responsive layout and no inline SVG. Use for visible Astro UI implementation; use astro-responsive-qa for browser validation.
---

# Astro UI implementation

Inspect the target project's Astro and Tailwind versions, `astro.config.*`, `tailwind.config.*` or CSS theme, nearby components, layouts, and route conventions before editing. Treat [Moduo project rules](../../rules/astro-project.md) as the design policy when the target project has adopted them. The reference project uses Astro 5, Tailwind 3, custom `xs`, `tablet`, `custom`, `laptop`, `desktop`, and `wide` breakpoints, and `mo-*` color tokens; do not assume those exist elsewhere.

## Establish the visual source

1. In an existing project, inventory its theme tokens, component patterns, and visual style. Search project references and connected design tools for a design even when the user has not explicitly mentioned one.
2. When a Penpot design exists, inspect it through the Penpot MCP. Map its colors, typography, spacing, radii, shadows, breakpoints, assets, and component states into the project's theme before writing page markup. Reconcile any difference from existing tokens with the requested scope. If the MCP or design is inaccessible, ask for access or a usable design reference instead of guessing its values.
3. When no design exists, show the user the existing conventions, if any, and ask whether to preserve or evolve them before laying out new UI. In a new project, ask for palette, type direction, references, and component preferences. Use `astro-design-tokens` to create or refine the theme from those answers, then implement the UI.

## Build

- Use semantic Astro markup. Extract each meaningful section, repeated block, and independently understandable part into a named component with a clear responsibility and explicit props. Keep pages readable as a composition of components. Follow the repository's layout, import aliases, formatting, and translation patterns.
- Mark the start and end of major visual sections or component groups in `.astro` markup with short HTML comments. Add brief, accurate code comments for responsibilities and non-obvious decisions; do not narrate trivial statements. Use descriptive variable and parameter names, never single-letter names.
- Keep pure helpers in the project's `utils` convention, API or CMS calls in services or adapters, and domain logic in the existing `lib` or equivalent. Move reusable behavior out of page templates and avoid duplicating it across components.
- Write the mobile layout with unprefixed Tailwind utilities, then adapt at the breakpoints configured by the target project. Prefer flexible widths, `min-w-0` where needed, sensible text wrapping, and content-driven heights over fixed device assumptions.
- Use Tailwind utilities for spacing, typography, color, layout, and ordinary states. Use a scoped style block or stylesheet when utilities would obscure a complex animation, selector, or browser behavior; document the reason. Avoid one-off inline `style` attributes for ordinary presentation.
- Consume colors and other reusable values through Tailwind theme tokens. Add missing tokens in the installed version's configuration (`tailwind.config.*` or CSS `@theme`) before using them. Do not scatter hex colors or repeated arbitrary values through `.astro` files and stylesheets.
- Do not add inline `<svg>` or use an icon component that emits it. Reference a standalone SVG asset through `<img>` or use an approved non-SVG asset. Preserve accessible text for meaningful graphics.
- Keep pages static by default. Add client-side scripts or islands only for actual interaction. If the project uses Astro's client navigation, initialize and clean up behavior so navigation does not duplicate listeners or leave stale state.
- Preserve all configured locales and relevant metadata when editing a shared component or route.

## Finish

Run the project's existing formatting, type, lint, and build checks that cover the change. For visible work, use `astro-responsive-qa` to inspect the rendered page at narrow and wide widths. Report the files changed, the responsive widths inspected, and any check that could not run.

Consult current [Astro components](https://docs.astro.build/en/basics/astro-components/), [Astro islands](https://docs.astro.build/en/concepts/islands/), and the installed version's [Tailwind responsive documentation](https://tailwindcss.com/docs/responsive-design) when an API or breakpoint behavior is uncertain.
