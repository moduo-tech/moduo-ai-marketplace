# Shared QA report format

Use this structure for functional, responsive, accessibility, performance, localization, SEO, and automated-test reviews. Keep reports concise but reproducible; omit a section only when it is irrelevant.

## Specialist report

- **Area and result:** functional, responsive, accessibility, performance, localization, SEO, or automated tests; `pass`, `issues found`, or `blocked`. `Pass` applies only to the scope actually checked.
- **Environment and scope:** base URL or build, browser and viewport where relevant, locale, routes, user journeys, and commands checked.
- **Findings:** stable ID such as `FUNC-01`, `A11Y-01`, or `PERF-01`; severity (`critical`, `high`, `medium`, `low`); route and component; steps or check; expected versus observed result; evidence (screenshot, trace, metric, output, or rendered HTML); concrete fix. Mark uncertain expectations as UX questions rather than confirmed defects.
- **Coverage and limits:** what worked, what was not tested, and environmental blockers. Distinguish `failed`, `not run`, and `blocked`.
- **Conclusion:** impact on the tested journeys and recommended next action. Never infer production readiness from a partial or source-only review.

## Consolidated report

The QA coordinator waits for all requested specialist reports, then creates one final report containing:

1. **Scope and result by area:** a compact table for functional, responsive, accessibility, performance, localization, SEO, and automated tests with status and coverage. State which areas were omitted for a focused review.
2. **Prioritized findings:** merge duplicate findings about the same root problem, retain every specialist ID and its evidence, and order by impact on real user journeys.
3. **Production readiness:** `ready`, `ready with known limitations`, or `not ready`, with the specific evidence supporting that judgment. A critical defect, broken core journey, or untested core journey prevents `ready`.
4. **Remaining gaps and next steps:** blocked checks, untested routes, and the smallest concrete fixes or reruns needed.

Do not turn an unrun check into a pass. If a specialist could not run, say so in its row and in the final conclusion. Preserve disagreements or uncertainty instead of silently choosing a favorable result.
