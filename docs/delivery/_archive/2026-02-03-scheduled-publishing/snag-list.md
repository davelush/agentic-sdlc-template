---
type: process
last-verified: 2026-02-18
---

# Scheduled Publishing — Snag List

**Status:** Closed at delivery (2 open snags carried — see below)
**Owner:** A. Writer
**Created:** 2026-02-11
**Related:** [implementation-plan.md](./implementation-plan.md)

Snags found during build and review. Open items at close are carried, not silently dropped — they are named in the [delivery review](./delivery-review.md).

## Snags at a glance

| # | Snag | Area | Type | Status |
|---|------|------|------|--------|
| 1 | Editor sent local time without offset → posts published hours off | Frontend | Bug | Resolved |
| 2 | Concurrent publishers could double-publish a due post | Backend | Bug | Resolved |
| 3 | Past time on save did nothing until the next poll tick | Full-stack | Bug | Open |
| 4 | Dashboard showed the scheduled time in UTC, not the writer's zone | Frontend | UX | Open |
| 5 | "Schedule" option still showed on already-published posts | Frontend | UX | Resolved |

---

## Open

### 3. A past `publish_at` on save did nothing until the next poll tick

**Found:** 2026-02-14, manual testing with a time a few seconds in the past.

**Where:** `PATCH /posts/:id` validation + the publisher.

**Why it matters:** The brief's open question (publish-now vs reject for past times) was only half-answered. The UI rejects obviously-past times, but a time that slips into the past between form render and submit is accepted and then waits up to a poll interval before going live — surprising, and briefly leaves a "scheduled for the past" post.

**Proposed resolution:** On the server, treat a `scheduled` request whose `publish_at` is already in the past as publish-now (flip straight to `published`). Small, but wasn't in this cycle's scope once the happy path landed.

### 4. Dashboard rendered the scheduled time in UTC

**Found:** 2026-02-15, review.

**Where:** dashboard post row.

**Why it matters:** The editor converts to the writer's zone but the dashboard badge was wired to the raw UTC value, so the two surfaces disagreed. Confusing, not incorrect data.

**Proposed resolution:** Route the dashboard badge through the same local-time formatter the editor uses.

---

## Resolved

### 1. Editor sent local wall-clock time without an offset

**Found:** 2026-02-12, first end-to-end test — a post scheduled for 09:00 published at 04:00.
**Resolved:** 2026-02-12, same session.

**What it was:** The time picker emitted a naive local datetime string; the server read it as UTC, so the post published at the writer's-offset-worth of hours early.

**Resolution:** The client now sends ISO-8601 with an explicit offset; the server converts to UTC on the way in. Added a test asserting a non-UTC writer's 09:00 lands at the right instant.

### 2. Two publisher instances could publish the same post twice

**Found:** 2026-02-13, while reasoning about running more than one app instance.
**Resolved:** 2026-02-13.

**What it was:** The publisher selected due posts and updated them in separate statements; two instances could both select the same row before either updated it.

**Resolution:** Select due rows with `FOR UPDATE SKIP LOCKED` inside the publish transaction, and re-check `status = 'scheduled'` before flipping. Idempotent and multi-instance safe. See [ADR-0001](../../../current-reality/decisions/adr-0001-publishing-trigger.md).

### 5. "Schedule" appeared on already-published posts

**Found:** 2026-02-15, review.
**Resolved:** 2026-02-16.

**What it was:** The Publish menu didn't account for `published` posts, offering to schedule something already live.

**Resolution:** Hide the Schedule option unless the post is a `draft`.
