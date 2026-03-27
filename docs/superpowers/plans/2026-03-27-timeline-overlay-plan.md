# Timeline Layout & Modal Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Align the timeline axis/selections to computed dates, keep the responsive line centered so cards stack neatly, and always show the detail view as a centered modal.

**Architecture:** The changes live entirely in `index.html`, where the inline CSS controls both desktop and mobile layouts and the embedded script manages event positions and modal behavior. We will adjust the JS helpers to compute the axis span, tweak the CSS for centered responsive stacking plus a consistent modal, and document the behavior in `docs/maintenance.md`.

**Tech Stack:** HTML, inline CSS, vanilla JavaScript.

---

### Task 1: Update timeline positioning logic

**Files:**
- Modify: `index.html:713-810`

- [ ] **Step 1: Compute the timeline span through the start of next year**

```js
const timelineStart = new Date("2020-01-01T00:00:00");
const getTimelineEnd = () => {
  const now = new Date();
  return new Date(`${now.getFullYear() + 1}-01-01T00:00:00`);
};
```

- [ ] **Step 2: Update `setEventPositions` to use `getTimelineEnd` and clamp against the safe bounds**

```js
const setEventPositions = () => {
  const timelineEnd = getTimelineEnd();
  const startMs = timelineStart.getTime();
  const endMs = timelineEnd.getTime();
  const spanMs = Math.max(endMs - startMs, 1);
  const safeBounds = getSafePositionBounds();

  events.forEach((event) => {
    const eventMs = new Date(`${event.dataset.date}T00:00:00`).getTime();
    const normalized = ((eventMs - startMs) / spanMs) * 100;
    const position = clamp(normalized, safeBounds.min, safeBounds.max);
    event.style.setProperty("--position", position.toFixed(4));
  });
};
```

- [ ] **Step 3: Ensure the computed end causes the axis labels to stay connected**

No DOM edits—relying on the clamped `--position` updates above ensures that the farthest items sit next to the axis badges, so mention in plan that no HTML change is needed.

### Task 2: Center timeline line and modal across breakpoints

**Files:**
- Modify: `index.html:171-238`, `index.html:480-680`

- [ ] **Step 1: Adjust the desktop/mobile `.timeline-track` pseudo-line**

```css
.timeline-track::before {
  left: 50%;
  right: 50%;
  transform: translateX(-50%);
  width: 4px;
  height: calc(100% - 2rem);
}
@media (min-width: 681px) {
  .timeline-track::before {
    left: 1.4rem;
    right: 1.4rem;
    width: auto;
    height: 4px;
    transform: none;
  }
}
```

- [ ] **Step 2: Update mobile event layout so cards stack around the centered line**

```css
@media (max-width: 680px) {
  .timeline-events {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 0.6rem;
  }
  .event { position: relative; width: 100%; display: flex; }
  .event-card { width: 100%; }
  .event.top .event-card { justify-self: flex-end; }
  .event.bottom .event-card { justify-self: flex-start; }
}
```

- [ ] **Step 3: Keep the modal centered on all screens**

```css
.modal-backdrop {
  align-items: center;
  justify-content: center;
}
.modal {
  width: min(560px, 100%);
  max-height: calc(100vh - 2rem);
  border-radius: 18px;
}
@media (max-width: 680px) {
  .modal {
    width: min(560px, calc(100% - 2rem));
    max-height: calc(100vh - 3rem);
  }
}
```

Remove any existing mobile overrides that set `width:100%` and `height:100%`, ensuring both breakpoints use centered modal styles.

### Task 3: Document the new behavior

**Files:**
- Modify: `docs/maintenance.md:17-30`

- [ ] **Step 1: Describe computed axis span and centered modal**

Add/update bullets to note:
1. Axis endpoints now derive from 2020 and `current year + 1` so the badges stay connected.
2. Mobile layout moves the line to the center and allows cards to stack on both sides when space is limited.
3. The detail view always opens in a centered modal overlay, even on mobile (no fullscreen takeover).

No further steps required; this keeps documentation aligned with UI behavior.
