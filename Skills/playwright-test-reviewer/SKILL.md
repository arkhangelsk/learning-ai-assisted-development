---
name: playwright-test-reviewer
description: Review Playwright test code against official best practices. Use when user shares test files (.spec.ts, .test.ts), Page Objects, fixtures, or config for review or improvement.
license: MIT
---

# Playwright Code Review

Review Playwright test automation code against official best practices from https://playwright.dev/docs/best-practices.

Read `references/patterns.md` for the full catalog of good/bad patterns and examples before giving feedback.

## Review Process

1. Identify which category the code falls into (test specs, Page Object Models, fixtures, config)
2. Check against the review checklist below
3. Structure feedback by severity, with refactored examples

## Review Checklist

When reviewing Playwright code, check for:

- No `waitForTimeout()` — replace with specific waits (element visibility, response, URL)
- No `waitForLoadState('networkidle')` — use element or response waits instead
- User-facing locators (`getByRole()`, `getByLabel()`) over CSS/XPath selectors
- Web-first assertions (`toBeVisible()`, `toHaveText()`, `toHaveCount()`) over manual checks
- Fixtures over `beforeEach` with mutable variables
- No non-null assertions (`!`) — validate or provide defaults
- `test.skip()` / `test.fixme()` over `test.fail()` for known bugs
- Locator filtering (`.filter()`, `.nth()`) over `.all()` with manual loops
- Descriptive test names that explain the expected behavior
- Test tags for filtering (`@smoke`, `@slow`, `@critical`)
- Constants over hardcoded magic numbers and strings
- Proper error handling — no silent `.catch(() => {})`
- `baseURL` validated, not accessed via non-null assertion
- Page Object methods that add business value, not thin wrappers around single clicks

## Locator Priority

Rank user-facing locators in this order:

1. `page.getByRole()` — ARIA role, name, state
2. `page.getByLabel()` — form fields by label
3. `page.getByPlaceholder()` — inputs by placeholder
4. `page.getByText()` — non-interactive text
5. `page.getByTestId()` — explicit test IDs (last resort)

Flag any use of `.locator('.class')`, `#id`, or positional `.nth()` without filtering as a code smell.

## Response Format

Structure review feedback as:

1. **Critical Issues** — Must fix: broken tests, anti-patterns, flaky test risks
2. **Performance Issues** — Unnecessary waits, missing parallelization
3. **Maintainability Issues** — Poor naming, missing structure, tight coupling
4. **Suggestions** — Nice-to-have improvements

For each issue, show the problematic code, the improved alternative, and a brief explanation of why it matters. End with a complete refactored example when the changes are substantial.
