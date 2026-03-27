# Portfolio Maintenance Guide

Last updated: 2026-03-27

## How to update safely

1. Refresh source data from the LinkedIn profile and capture an updated snapshot date.
2. Update role titles, dates, and education details in `index.html`.
3. Keep content factual and avoid adding unverifiable claims.
4. If new claims are added, first add supporting source notes to `docs/content.md`.

## Content style rules

- Prefer role-based experience language from LinkedIn experience sections.
- Keep dates explicit in `mm/yy` format when month precision is used (e.g., "08/25 - Present").
- Keep Fishbowl internship timing anchored to August 2024 through May 2025; do not shift it to July unless the source snapshot changes.
- Treat connection counts and similar metrics as snapshots.

## UI behavior notes

- Theme colors in `index.html` follow the browser or OS preference via `prefers-color-scheme` (light/dark).
- The timeline area now renders as yearly rows of fixed-size monthly blocks from January 2023 through December of the current calendar year; each row wraps at January, future months in the current year stay visible as empty placeholders, each active block is color-coded by experience category, and the current (Now) month remains highlighted.
- Each year row stays horizontally scrollable on narrow viewports, swaps to tighter heights, and keeps the legend/labels in place while preserving the glassy background treatment.
- Clicking any block opens the detail modal, which still shows the original description and tags and remains centered at every breakpoint instead of flipping to a fullscreen takeover.
