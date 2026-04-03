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
- The timeline area now renders as yearly rows of fixed-size monthly blocks from January 2023 through December of the current calendar year; each row wraps at January, future months in the current year stay visible as empty placeholders, each active block is color-coded by experience category, the tile itself shows only the month label, and the current (Now) month remains highlighted.
- Each year row stays horizontally scrollable on narrow viewports, swaps to tighter heights, and keeps the legend/labels in place while preserving the glassy background treatment.
- Clicking any block opens the detail modal, which still shows the original description and tags and remains centered at every breakpoint instead of flipping to a fullscreen takeover.
- Timeline dates should be parsed as local calendar dates in `index.html`; using bare `new Date('YYYY-MM-DD')` can shift first-of-month entries into the prior month on negative-offset time zones.

## Catchup page notes

- The `/catchup` page is a static route at `catchup/index.html`.
- The page expects a `code` query parameter and writes live geolocation telemetry to Firestore.
- The page treats the session as tracked when the Firestore document's `observedAt` value is within the last minute.
- When tracked, the page writes a full telemetry update after movement greater than 10 meters or a speed change of at least 1 m/s, including transitions to or from 0 m/s.
- When not tracked, the page writes a full telemetry update after movement greater than 50 meters or a speed change of at least 10 m/s, including transitions to or from 0 m/s.
- When movement and speed stay below the threshold, the page sends heartbeat updates every 3 minutes while tracked and every 30 minutes while idle.
- Full telemetry updates write `currentLatitude`, `currentLongitude`, `currentSpeedMPS`, `currentHeadingDegrees`, `tracked`, `distance`, `kind`, `sentAt`, and `userID` into `catchupTelemetry/{code}`.
- Heartbeats write `tracked`, `distance`, `kind`, `sentAt`, and `userID` into the same document.
- `tracked` mirrors whether the web session is actively considered tracked from the existing `observedAt` freshness check.
- `distance` is written as `null` on the web page unless a future target-distance source is available; this keeps older documents readable without inventing a value.
