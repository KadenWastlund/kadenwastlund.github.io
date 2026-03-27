# Experience Timeline Block Implementation Plan

> **For agentic workers:** This plan replaces the previous axis/rail layout with the monthly block grid design described in the latest spec right above. Steps use checkbox (`- [ ]`) syntax for tracking progress if you choose to follow the steps sequentially.

**Goal:** Render the experience timeline as year-wrapped rows of fixed-size color-coded monthly blocks that cover January 2023 through December of the current year, leaving future months visible as empty placeholders, and keep the detail modal centered across breakpoints.

**Architecture:** Everything remains within `index.html`. The inline CSS now styles the new `.timeline-grid`, the JS builds the block collection and legend from the static entry data, and the modal logic still handles block clicks. Documentation updates follow in `docs/content.md` and `docs/maintenance.md`.

---

### Task 1: Build the monthly block data model

**Files:** `index.html`

- [ ] Compute the timeline span once from 2023-01-01 through December 31st of the current year so the current year always reserves space for future months.
- [ ] Iterate month-by-month to gather an array of `Date` objects, keeping a reference to the latest entry that was active during each month (prefer whichever entry started most recently when ranges overlap).
- [ ] Store the entry metadata (title/subtitle/description/tags/type/range) in a structured array so the rendering logic can reuse it for both block classes and modal content.

### Task 2: Style the block grid

**Files:** `index.html`

- [ ] Replace the `.timeline-track` layout with the `.timeline-grid` structure, including the header labels, year-wrapped block rows, and color legend.
- [ ] Add CSS variants for each entry type (`education`, `program`, `internship`, `occupation`, `idle`, `future`) plus a `.timeline-block--current` highlight, and keep the scroll track responsive across breakpoints with fixed-size month cells.
- [ ] Keep the legend in place and ensure each year row remains accessible when horizontal scrolling is required on phones.

### Task 3: Wire blocks to the modal

**Files:** `index.html`

- [ ] Render `button` elements for every month, annotate each block with `data-month-label`, and assign `aria-label`s that describe the month plus the entry details.
- [ ] Highlight the block representing the current month on load, and make each active block open the modal overlay with the matching entry detail (or a fallback message when no entry covers the month).
- [ ] Reuse the existing modal markup, keeping the backdrop/ESC/backdrop-close behaviors untouched so it remains centered on all screen sizes.

### Task 4: Update documentation

**Files:** `docs/maintenance.md`, `docs/content.md`

- [ ] Rewrite the UI behavior notes and content constraints to describe the monthly block timeline, the legend, and the modal-driven detail view.
- [ ] Mention the new timeline approach in the supporting claims so maintainers know to keep the block data synced with LinkedIn snapshots.
