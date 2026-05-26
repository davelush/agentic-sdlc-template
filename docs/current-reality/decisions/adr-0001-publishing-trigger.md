---
type: reality
last-verified: 2026-02-18
---

# ADR 0001: Publishing trigger — poll for due posts

**Status:** Accepted
**Date:** 2026-02-13
**Increment:** 2026-02-03-scheduled-publishing

## Context

Scheduled publishing needs something to flip a post from `scheduled` to `published` when its `publish_at` time arrives, without anyone pressing a button. Three mechanisms were on the table:

- **(A) Poll** — a scheduled task runs once a minute, selects posts whose `publish_at` is due, and publishes them.
- **(B) Job queue** — when a post is scheduled, enqueue a delayed job for its exact publish time.
- **(C) Database-driven** — use a database scheduler extension (e.g. `pg_cron`) or `LISTEN/NOTIFY` to fire at the time.

Our appetite allows up to ~1 minute of latency, and we have no queue infrastructure today. We also expect the app to run on more than one instance, so whatever we choose must not double-publish.

## Decision

**Option A — poll once a minute.** A scheduled task selects due posts and publishes each inside a transaction, using `SELECT ... FOR UPDATE SKIP LOCKED` and re-checking `status = 'scheduled'` before flipping to `published`. This makes publishing idempotent and safe to run on multiple instances concurrently.

## Consequences

- **Simple, no new infrastructure.** We avoid standing up and operating a job queue for a single feature.
- **Up to ~60s latency** between the scheduled time and going live — within appetite. If we ever need sub-minute precision, this ADR should be revisited.
- **Idempotency is mandatory and built in** — the row lock plus the status re-check mean a post is published exactly once even if two instances poll simultaneously or a tick overlaps the previous one.
- **Scales until it doesn't.** The poll scans an indexed `(status, publish_at)` predicate; fine at our volume. A very large backlog of due posts per tick would be the signal to move to option B.
