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
- Timeline markers are positioned proportionally to elapsed time from January 2020 to the current date ("Now"), rather than evenly spaced by item count.
- Timeline cards alternate above/below the center line on desktop and can shift into additional vertical lanes when dates are close, then stack into a vertical rail on mobile (`max-width: 680px`).
- Timeline axis labels (`2020`, `Now`) render as small badges with short pointer ticks at the start and end of the timeline rail.
