---
type: delivery-plan
status: delivered
last-verified: 2026-02-18
---

# Scheduled Publishing — Implementation Plan

**Source brief:** [prd-pitch.md](./prd-pitch.md)

## 1. Framing decisions (locked in conversation)

- A post has a single lifecycle status; scheduling is a state of that lifecycle, not a separate table.
- All times are stored in UTC. The writer's timezone comes from their profile (already captured at signup), not the browser, so it's stable across devices.
- The trigger mechanism is decided separately — see [adr-0001-publishing-trigger.md](../../../current-reality/decisions/adr-0001-publishing-trigger.md).

## 2. Where this fits in the current product

Extends the existing `posts` model and the editor's Publish action. No new page, no new nav. The public post-rendering path is unchanged — a scheduled post simply isn't publicly visible until its status flips to `published`.

## 3. User journey, mapped to the system

| Brief step | System |
|---|---|
| Choose Schedule | Editor Publish menu → `Schedule` opens the time picker |
| Pick a time | `PATCH /posts/:id` with `{ status: "scheduled", publish_at }` (UTC) |
| See it scheduled | Dashboard + editor read `status` + `publish_at`, render in the writer's zone |
| Reschedule / unschedule | `PATCH /posts/:id` with a new `publish_at`, or `{ status: "draft" }` |
| It publishes itself | The publisher flips `scheduled` → `published` when `publish_at` is due |

## 4. Data model

Extend `posts`:

- `status`: enum `draft | scheduled | published` (stored lowercase string). Was effectively `draft | published` before.
- `publish_at`: `timestamptz`, nullable — set only while `scheduled`.
- `published_at`: `timestamptz`, nullable — set when the post actually goes live (carried over from today's publish path).

**Not introducing:** a separate `scheduled_posts` table, a jobs table, or any recurrence fields.

## 5. Frontend

- **Publish menu** — split the existing Publish button into *Publish now* / *Schedule for later*.
- **Time picker** — date + time, defaulted to the writer's profile timezone, with the zone shown explicitly.
- **Scheduled badge** — on the dashboard row and the editor header: "Scheduled for &lt;local datetime&gt;".
- **Reschedule / unschedule** — actions on a scheduled post.
- Reuse the existing post-status pill component; add the `scheduled` variant.

## 6. Backend

- `PATCH /posts/:id` accepts `status` and `publish_at`; validates that `scheduled` requires a future `publish_at`.
- A **publisher** that finds due scheduled posts and publishes them — mechanism per [ADR-0001](../../../current-reality/decisions/adr-0001-publishing-trigger.md). Must be idempotent and safe to run on more than one instance.
- Endpoint → service → repository layering as per the constitution; the publish transition lives in the service.

## 7. Cross-cutting concerns

- **Timezones:** the API speaks UTC ISO-8601 with offset; the client converts to/from the writer's zone. No local time is ever persisted.
- **Visibility:** the public read path filters to `status = 'published'` — a scheduled post must not be reachable by URL before its time.

## 8. Phasing and ownership

### Phase 0 — Schema + state
- Migration: add `status` enum value `scheduled`, add `publish_at`. Backfill existing posts.
- **Gate.**

### Phase 1 — Schedule, reschedule, unschedule (no auto-publish yet)
- Editor Publish menu, time picker, dashboard badge, `PATCH` validation.
- A scheduled post sits in state but does not yet go live on its own.
- **Gate.**

### Phase 2 — The publisher
- Implement the trigger from ADR-0001; idempotent; multi-instance safe.
- **Gate.**

### Phase 3 — Polish
- Past-time handling, empty/loading states, copy.
- **Gate.**

## 9. Risks and rabbit holes

- **Double publish** if the trigger runs concurrently on two instances — mitigated by row locking (see ADR-0001).
- **Timezone leakage** — mitigated by storing UTC only and converting at the edges.
- **Scope creep into a calendar / recurrence** — explicitly out of scope.

## 10. Deliberate deviations from the brief

- Added **unschedule** (back to draft) as a first-class action. The brief implied edit/reschedule; unschedule turned out to be the natural third action and was cheap.

## 11. What this plan deliberately does not include

Recurrence, social/email-on-publish, best-time suggestions, a calendar view, per-section scheduling.
