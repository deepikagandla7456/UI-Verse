# feat(storybook): migrate components into Storybook with isolation and snapshot tests (Issue #1270)

## Summary
- Migrates component demos into Storybook so they can be developed and reviewed in an isolated environment.
- Uses shadow-root based story rendering to keep styles scoped and prevent component leakage across previews.
- Adds Playwright snapshot tests for the migrated stories to catch visual regressions.
- Wires CI to build Storybook and run the snapshot suite.

## What I changed
- Added Storybook tooling to `package.json`
  - `storybook` to run the Storybook dev server.
  - `build-storybook` to build static Storybook output.
  - `test:storybook` to run the Playwright snapshot suite.
- Added Storybook config
  - `.storybook/main.js` configures `@storybook/html-vite` and exposes local static assets.
  - `.storybook/preview.js` sets fullscreen layout defaults.
- Added isolated story helpers and stories
  - `stories/storybook-utils.js` wraps stories in a shadow root.
  - `stories/animation-library.stories.js` migrates the animation library UI into Storybook.
  - `stories/testimonials-carousel.stories.js` migrates the testimonials carousel into Storybook.
- Added Storybook snapshot tests
  - `tests/storybook/storybook-snapshots.spec.ts` compares Storybook story screenshots.
  - Snapshot baselines were generated for the migrated stories.
- Added CI support
  - `.github/workflows/storybook-snapshots.yml` builds Storybook and runs the snapshot suite in GitHub Actions.

## Why
- Storybook provides a stable isolated environment for reviewing components without page-level interference.
- Shadow-root isolation prevents accidental global style leakage and keeps stories closer to production-safe rendering.
- Snapshot tests make visual regressions detectable in CI before merge.

## Verification
- Ran `npm run build-storybook` successfully.
- Ran `npm run test:storybook -- --update-snapshots` successfully.
- Both migrated stories passed their snapshot tests:
  - `Motion/Animation Library`
  - `Components/Testimonials Carousel`

## Notes
- The current migration focuses on two representative component families to establish the pattern.
- The Storybook setup can be extended to additional component pages by adding more `.stories.js` files and snapshot cases.
