# Experience Timeline Block Layout

## Background
The timeline strip is no longer a line with branch cards—it now renders as a horizontal sequence of monthly blocks that stretch from January 2020 through the start of the next calendar year. Each block collates whatever entry was active during that month, so there is always a deterministic color and name associated with the block, and the “Now” label is a living indicator tied to the current calendar year.

## Requirements
1. Render every month between January 2020 and January of the year after today as a block; the container should scroll horizontally rather than stacking the months vertically. The “Now” marker must display the current year and align with the month containing today.
2. Color-code the blocks by entry type (education/program/internship/occupation) and highlight the current month so readers see where the timeline currently sits. Months without a listed entry should have a muted block treatment but remain clickable.
3. Provide a legend that explains the color palette and keeps its text visible on narrow viewports.
4. Keep the detail modal overlay intact: clicking a block opens the modal, showing the entry title, subtitle, range, description, and tags. The modal stays centered at every breakpoint and does not switch to a fullscreen takeover on mobile.

## Approach
- JavaScript computes the fixed span from January 2020 to the start of next year and iterates month-by-month to create the block grid. Each block looks up the most recent entry whose span covers the month (most recent start date wins), and a legend explains the colors.
- Blocks are constructed as buttons inside a scrollable, glassy track; `aria-label`s communicate the month plus the entry name/subtitle, and the current-month block gets an additional highlight class.
- The modal reuses the existing overlay markup but now accepts block clicks to load the proper description/tag data, with close/backdrop/ESC handling unchanged.
- Responsive tweaks reduce block height and tighten gaps below 680px while keeping the legend legible and the scroll track accessible.

## Testing
- Resize from wide desktop down to small mobile to confirm the monthly blocks keep their horizontal flow, shrink as expected, and remain scrollable while the legend stays visible.
- Cycle through each block (especially the current month) to ensure the modal opens with the right title/subtitle/date/tags and that the color matches the type of entry that covers that month.
- Verify that months with no events still open the modal with a fallback message so nothing is left as dead space.

## Next checkpoints
- Watch for additional entries or gaps in the timeline data so the month generator stays accurate after future updates.
- Keep the legend colors and block palette in sync with design guidelines whenever the token palette evolves.
