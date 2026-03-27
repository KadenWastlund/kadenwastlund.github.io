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
- Timeline markers are positioned proportionally along the span between January 2020 and January of the year after today, with edge clamping keeping the first/last cards fully visible.
- Timeline cards alternate above/below the center line on desktop with extra vertical lanes when dates crowd together; at `max-width: 680px` the track turns into a centered vertical rail and cards stack to both sides of the line.
- Timeline axis labels (`2020`, the year after the current year) render as small badges at the computed start/end positions so they stay connected to the actual rail.
- Timeline cards show title and subtitle only; detailed copy and tags are shown in a modal when a card is clicked.
- The detail modal is centered at every screen size instead of switching to a full-screen experience on mobile.
