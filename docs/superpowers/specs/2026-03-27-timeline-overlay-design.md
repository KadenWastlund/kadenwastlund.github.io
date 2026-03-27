# Experience Timeline Block Layout

## Background
The timeline strip is no longer a line with branch cards—it now renders as year-wrapped rows of fixed-size monthly blocks that extend from January 2023 through December of the current calendar year. Each block collates whatever entry was active during that month, future months in the current year remain visible as empty placeholders, and the “Now” label is a living indicator tied to the current calendar year.

## Requirements
1. Render every month between January 2023 and December of the current year as a block; months should group into year rows so the timeline wraps at each January. The “Now” marker must display the current year and align with the month containing today.
2. Color-code the blocks by entry type (education/program/internship/occupation) and highlight the current month so readers see where the timeline currently sits. Months without a listed entry should have a muted block treatment, while future months in the current year should stay visible as empty placeholders and be non-interactive.
3. Provide a legend that explains the color palette and keeps its text visible on narrow viewports.
4. Keep the detail modal overlay intact: clicking a block opens the modal, showing the entry title, subtitle, range, description, and tags. The modal stays centered at every breakpoint and does not switch to a fullscreen takeover on mobile.

## Approach
- JavaScript computes the fixed span from January 2023 to the end of the current year and iterates month-by-month to create year-grouped block rows. Each block looks up the most recent entry whose span covers the month (most recent start date wins), and a legend explains the colors.
- Blocks are constructed as buttons inside scrollable, glassy year rows; `aria-label`s communicate the month plus the entry name/subtitle, and the current-month block gets an additional highlight class.
- The modal reuses the existing overlay markup but now accepts block clicks to load the proper description/tag data, with close/backdrop/ESC handling unchanged.
- Responsive tweaks reduce block height and tighten gaps below 680px while keeping the legend legible and the scroll track accessible.

## Testing
- Resize from wide desktop down to small mobile to confirm the monthly blocks wrap by year, shrink as expected, and remain scrollable within each row while the legend stays visible.
- Cycle through each active block (especially the current month) to ensure the modal opens with the right title/subtitle/date/tags and that the color matches the type of entry that covers that month.
- Verify that months with no events still open the modal with a fallback message, while future months remain visible but disabled so nothing is left as oversized dead space.

## Next checkpoints
- Watch for additional entries or gaps in the timeline data so the month generator stays accurate after future updates, and keep the placeholder months aligned with the current-year cutoff.
- Keep the legend colors and block palette in sync with design guidelines whenever the token palette evolves.
