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
- Treat connection counts and similar metrics as snapshots.

## UI behavior notes

- Theme colors in `index.html` follow the browser or OS preference via `prefers-color-scheme` (light/dark).
- The timeline area now renders as a horizontal series of monthly blocks from January 2020 through the start of next year; each block is color-coded by the active experience category, highlights the current (Now) month, and keeps the legend aligned with the visible track.
- The block grid stays scrollable on narrow viewports, swaps to tighter heights, and keeps the legend/labels in place while preserving the glassy background treatment.
- Clicking any block opens the detail modal, which still shows the original description and tags and remains centered at every breakpoint instead of flipping to a fullscreen takeover.
