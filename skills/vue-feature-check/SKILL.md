---
name: vue-feature-check
description: Verify changed Vue/Nuxt feature behavior, interaction states, accessibility, responsive layout, and supported locales before review or after a UI regression. Use for user-visible feature acceptance, not a general coding-style audit or a framework migration.
---

# Vue feature check

Follow the bundled [specialist execution boundaries](../coding-workflow/references/specialist-execution.md). This skill verifies feature acceptance; coding-guidelines owns formatting and Vue block/attribute ordering. Retain the active task's delivery approvals when fixes are requested.

## Establish the feature boundary

- Read the issue/acceptance criteria and relevant diff, components, composables, routes, state stores, API adapters, and tests. Identify supported Vue/Nuxt versions, component library, browser/viewport requirements, locales, and the real commands/fixtures for component and browser checks.
- Trace the user action to visible state and side effects. Include SSR/hydration or client navigation only when relevant. Follow existing props/emits/model contracts; tests must not replace validation or persistence adapters with a simpler behavior.
- Select applicable cases from the [acceptance matrix](references/acceptance-matrix.md). Link each selected case to a real requirement or changed risk, and note why unavailable relevant cases could not run. Use existing tests/fixtures instead of requiring Playwright or a scanner installation.

## Verify behavior and evidence

- Exercise the main journey and changed boundary states with deterministic local fixtures: success, empty results, pending, failure/retry, validation, disabled controls, and permission-dependent behavior as applicable. Inspect the rendered result and actual side effect; a successful click or HTTP response alone does not establish the expected UI outcome.
- Check form labels, accessible names, keyboard navigation, focus entry/return, modal dismissal, error announcements, and reachable controls as relevant. Run an existing accessibility scanner if available, then separately inspect interaction/focus behavior. A clean automated scan is not complete accessibility certification.
- Verify the project's supported locales, especially en-US/de-DE when both are supported: missing keys, fallback masking, interpolation/plurals, long text, dates/numbers, and language switching. Do not add a new locale solely for this check. Keep test selectors stable across languages through established role/label/test-id conventions.
- Inspect affected layouts at project-relevant narrow/wide viewports, zoom or long content where appropriate, and supported themes if affected. Capture reviewable screenshots only with synthetic/redacted data. Assertions and visual inspection complement each other; screenshots alone do not prove interactive behavior.
- Use the connected browser tools for interactive observations and the repository's existing test runner for automated checks. Prefer condition-based waits and retrying assertions over arbitrary sleeps or forced clicks that conceal actionability failures. Do not weaken assertions or update snapshots solely to make the test pass.
- Separate component-mocked evidence from real integration/browser evidence. Mocked API success does not verify authorization, backend persistence, or live provider behavior. If fixtures/server/browser access are unavailable, report the corresponding gap instead of treating lint, types, or static markup as a browser pass.

## Complete the check

- For each failure, record route/scenario, locale/viewport, expected versus observed behavior, and the smallest useful reproduction/evidence. Preserve existing behavior outside the approved change; route API/data-layer issues to their established owner rather than silently rewriting the frontend contract.
- When fixes are authorized, add or update a focused regression test at the appropriate component/browser seam, run it and relevant project checks, and recheck the affected state matrix. Do not duplicate the whole suite for an isolated correction.
- Return an acceptance verdict for the tested scope: verified, failed, or incomplete. Identify what ran, what was mocked, and what remains unverified. Keep artifacts in the project's existing test-output location; do not commit sensitive traces or transient browser state.

Version-specific references: [Playwright actionability](https://playwright.dev/docs/actionability) and [accessibility testing](https://playwright.dev/docs/accessibility-testing). These are guidance for projects using Playwright, not mandatory dependencies.
