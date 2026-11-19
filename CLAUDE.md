# Project: Presenter App (live workshop build)

This repo builds a presenter mode for `unbounded_contexts_slides/unbounded_contexts_slides.html`,
a single-file HTML slide deck. See `PLAN.md` for the feature scope and `commands.md` for the
command cheat sheet.

## Verify loop is required for every feature

A feature or bug fix is not done when the code looks right. It's done when a Playwright test
in `tests/` actually drives the real behavior in a real browser and passes.

- Before starting a feature, either extend an existing spec or add a new one in `tests/` that
  encodes the expected behavior (e.g. "pressing P hides the nav buttons").
- Run `npx playwright test` and confirm it's red for the right reason (the feature doesn't
  exist yet), then make it pass.
- A change isn't finished until `npx playwright test` is green. Don't declare something working
  from reading the diff alone.
- If a test fails in a way that isn't about the feature being built (like the pre-existing
  slide-counter bug), treat that as a real finding to report, not something to quietly work around.

## Coding style

- Small, intention-revealing names. Keep functions short and single-purpose. No commented-out code.
- Every new or changed function gets a JSDoc comment: a one-line summary of what it's meant to do,
  `@param` for each parameter, and `@returns` when it returns something. Explain the *why* when
  it isn't obvious (e.g. a workaround or a browser quirk), not a restatement of the code.
- In the slide deck HTML, keep new CSS and JS next to the related existing code, and match the
  surrounding style (indentation, naming, quote style).
- The deck stays a single self-contained HTML file: no new dependencies, CDN scripts or build step.
- Use `const`/`let`, never `var`. Don't add new globals; keep new code inside the existing `<script>`
  and reuse its helpers (`goTo`, `toggleNotes`, `toggleChrome`) rather than duplicating them.
- Keyboard shortcuts go in the existing `keydown` handler. They must ignore events with
  Ctrl/Cmd/Alt held (so browser shortcuts like Ctrl+P keep working) and events while focus is in
  an input or textarea.
- Playwright specs: one behavior per `test(...)`, named for what the user sees
  (e.g. "pressing P hides the nav buttons").
- Playwright locators: prefer `getByRole` / `getByText` over CSS selectors, and use web-first
  assertions (`await expect(locator).toBeVisible()`). Never use `waitForTimeout` or fixed sleeps.

## Commits

Never commit without being explicitly asked, each time. An earlier approval doesn't carry
forward to later changes in the same session.
