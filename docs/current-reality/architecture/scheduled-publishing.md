---
type: reality
last-verified: 2026-02-18
---

# Scheduled publishing

How a post goes live at a future time, in Inkwell today. (Worked-example doc — Inkwell is fictional.)

## Post lifecycle

A post's `status` is one of:

- `draft` — being written; not publicly visible.
- `scheduled` — has a future `publish_at` (UTC); not publicly visible until then.
- `published` — live; `published_at` is set.

Relevant `posts` columns: `status` (lowercase string enum), `publish_at` (`timestamptz`, set only while scheduled), `published_at` (`timestamptz`, set when it goes live).

## Scheduling

The editor's Publish action offers *Publish now* or *Schedule for later*. Scheduling sends `PATCH /posts/:id` with `{ status: "scheduled", publish_at }`. Times are handled in UTC end to end: the client sends ISO-8601 with an explicit offset, the server stores UTC, and both editor and dashboard render in the writer's profile timezone. Rescheduling is another `PATCH` with a new `publish_at`; unscheduling sets `status` back to `draft`.

## The publisher

A scheduled task runs **once a minute** and publishes due posts. Each tick, inside a transaction, it selects scheduled posts whose `publish_at <= now()` using `FOR UPDATE SKIP LOCKED`, re-checks `status = 'scheduled'`, then sets `status = 'published'` and `published_at = now()`.

This is **idempotent and multi-instance safe**: the row lock plus the status re-check guarantee a post is published exactly once even if two app instances poll at the same time or a tick overlaps the previous one. Publishing latency is bounded by the poll interval (~60s), which is within the feature's appetite.

The poll-versus-queue choice and its trade-offs are recorded in [ADR-0001](../decisions/adr-0001-publishing-trigger.md).

## Visibility

The public post-rendering path filters to `status = 'published'`, so a scheduled post is not reachable by URL before its time.

## Known gaps

Tracked as open snags from the delivery increment (not yet scheduled):

- A `publish_at` that becomes past *between* form render and submit is accepted and waits for the next poll tick, rather than publishing immediately.
- The dashboard badge renders the scheduled time in UTC while the editor renders it in the writer's zone.
