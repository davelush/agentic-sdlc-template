---
type: process
last-verified: 2026-02-18
---

# Scheduled Publishing — Delivery Review

**Status:** Closed
**Created:** 2026-02-18
**Source brief:** [prd-pitch.md](./prd-pitch.md)
**Implementation plan:** [implementation-plan.md](./implementation-plan.md)

Assessment of what shipped against the original brief, based on the increment's git history and the source tree.

---

## 1. How we achieved the brief

The wedge is built and end-to-end. A writer can schedule a post from the editor, see and change it, and the post goes live on its own.

- **Schedule from the editor** — the Publish button splits into *Publish now* / *Schedule for later*, opening a timezone-aware picker. Confirming sets `status = scheduled` and `publish_at` (UTC).
- **Visible, editable state** — dashboard and editor both show a Scheduled badge; reschedule and unschedule both work.
- **Reliable auto-publish** — a once-a-minute publisher flips due posts to `published`, within the ~1-minute appetite.
- **Idempotent** — concurrent publishers can't double-publish (snag #2), via row locking. See [ADR-0001](../../../current-reality/decisions/adr-0001-publishing-trigger.md).
- **Timezone-correct** — UTC stored, local rendered; the early off-by-hours bug (snag #1) is fixed and covered by a test.

The won't-do list held: no recurrence, no social/email, no best-time suggestions, no calendar.

## 2. Where we fell short

- **Past-time handling is half-done** (snag #3, open): the UI rejects obviously-past times, but a time that slips into the past on submit waits for the next poll tick rather than publishing immediately.
- **Dashboard timezone display** (snag #4, open): the dashboard badge still renders UTC while the editor renders local — the surfaces disagree.

Both are carried into the open-snag inventory rather than fixed in-cycle.

## 3. Where we deviated from the brief and added beyond it

### Deliberate deviations (decided in the plan)
- Added **unschedule** (scheduled → draft) as a first-class action; the brief only implied edit/reschedule.

### Additions not in the brief
- None of substance — scope held.

## Close-out

**Graduated into `current-reality/`:**
- [`decisions/adr-0001-publishing-trigger.md`](../../../current-reality/decisions/adr-0001-publishing-trigger.md) — the poll-vs-queue decision.
- [`architecture/scheduled-publishing.md`](../../../current-reality/architecture/scheduled-publishing.md) — how scheduled publishing works today (states, the publisher, idempotency).

**Archived:** this folder (pitch, plan, snag list, this review) is the process record.

**Open-snag position at close:** two open (#3, #4), both small and named above. They are acknowledged debt with no scheduled fix — the honest state, and exactly the "when does the snag list get worked?" question the [playbook](../../../current-reality/practices/playbook.md) leaves open.
