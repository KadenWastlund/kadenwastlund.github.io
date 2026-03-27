# Timeline Layout & Modal Design

## Background
The timeline section currently hard-codes axis endpoints, mixes desktop/mobile treatments for the detail modal, and relies on responsive CSS that leaves the vertical line stuck to one side on narrow screens. We need to fix the positioning, spacing, and modal behavior so the desktop/mobile experiences match the desired timeline aesthetic.

## Requirements
1. Timeline rail start must anchor at January 2020 and the end must move with the current date such that the axis labels ("2020" and "Now") always sit on the physical timeline line, and the computed end timeline value should be "current year + 1" so the rail keeps extending to the future.
2. Desktop cards continue to alternate above/below with lane offsets, but crowded events should add extra lanes to avoid overlap without manual markup. When the layout shrinks, events should stack into a single, centered column with the timeline line centered horizontally so cards use both sides.
3. Timeline detail always opens inside a centered modal overlay regardless of screen width—no full-screen takeover on mobile. The modal should keep the same title/body layout used on desktop.
4. Documentation (especially `docs/maintenance.md`) must describe the new behavior so future updates remain aligned with the UI.

## Approach
### Computed timeline positions
- JavaScript already parses each event's `data-date`. Extend `setEventPositions` to determine the span between `2020-01-01` and a `now` value representing the current year + 1 (e.g., if now is 2026, treat the span as ending 2027-01-01). This moves the “Now” label position to the real end of the rail.
- The axis badges remain purely CSS positioned at the track edges, but their rendered text stays `2020` / `Now`; the timeline markers will align because the computed `--position` for the last event can reach the clamped maximum.
- When the window resizes or on orientation change, recalc `setEventPositions` so the marker positions stay accurate.

### Responsive stacking
- Desktop (above 680px) keeps `event.top`/`event.bottom` lanes with `--lane-level` offsets. Extend `assignLaneLevels` to re-run when layout changes and clamp placements to avoid overflow.
- Mobile (≤680px) will treat `.timeline-track` as a centered vertical layout: remove the CSS that pins the rail to one side and instead set `left: 50%` on the pseudo-axis line and `transform: translateX(-50%)`, so the line sits in the center of the column. The cards become full-width, stacking vertically, but we also keep an alternating `align-self` using the existing `top`/`bottom` classes for left/right relative placement if needed.
- Add CSS to allow cards to flow into a `grid` with `gap` so crowded sections just stack. On desktop we keep the lane computations to add vertical offsets and avoid overlaps.

### Modal overlay
- Stop forcing `.modal` to width/height 100% on mobile. Instead, keep the default modal size (max-width 560px, centered) and use CSS/JS to ensure the backdrop remains visible. Remove or override the mobile-specific CSS in the media query so mobile still uses the centered dialog.
- Keep the existing modal open/close logic (click card → open overlay, ESC/backdrop closes). No full-screen wrapper on small screens.

### Documentation updates
- Update `docs/maintenance.md` to describe the new timeline axis logic (computed from 2020 to current year + 1), the responsive centered line on mobile, and the fact that the detail view now always appears as a centered modal instead of taking up the entire viewport.

## Testing
- Resize the browser from wide to narrow to ensure the line stays centered and cards no longer push against one side.
- Confirm the axis badges align with the timeline track on wide and narrow layouts, and the “Now” marker extends with the computed year + 1.
- Open each timeline card on desktop and mobile to verify the modal is centered and works everywhere.
- Run Lighthouse or plain visual checks once the layout changes are applied.

## Next checkpoints
- Apply CSS/JS updates as described.
- Update documentation.
- Verify behavior manually in Chrome/Firefox and at mobile breakpoints.
