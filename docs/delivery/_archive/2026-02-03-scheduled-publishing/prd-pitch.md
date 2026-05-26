---
type: process
last-verified: 2026-02-03
---

# Shape Up Brief: Inkwell — Scheduled Publishing

**Product:** Inkwell — a lightweight blogging platform (fictional; used here to demonstrate the method)

## 1. Problem

Writers on Inkwell finish a post in the evening but want it to go live at a better time — Tuesday at 9am, when their readers are around. Today the only way to publish is to hit **Publish** in the moment. So they either stay up to publish manually at the right time, or they publish whenever they happen to finish and accept worse reach. The work is done; the timing is the unmet need.

## 2. Appetite

One cycle. This is a focused, high-demand quality-of-life feature, not a content calendar. The smallest thing that lets a writer set a future time, walk away, and trust the post will go live. We are not building a scheduling *product* — just reliable deferred publishing for a single post.

## 3. Solution

In the editor, the **Publish** action gains a second option: **Schedule**. The writer picks a date and time (in their own timezone). The post moves into a **Scheduled** state, shows on their dashboard with the go-live time, and can be edited, rescheduled, or unscheduled until then. At the scheduled time the post goes live automatically, exactly as if they'd pressed Publish.

### Core user flow

1. **Write the post** — as today, in the editor; the post is a `draft`.
2. **Choose Schedule** — the Publish button opens a small menu: *Publish now* / *Schedule for later*.
3. **Pick a time** — a date + time picker, defaulted to the writer's timezone. Confirm.
4. **See it scheduled** — the post shows on the dashboard with a "Scheduled for Tue 9 Feb, 09:00" badge; the editor shows the same.
5. **Change your mind (optional)** — edit the post, reschedule to a new time, or unschedule back to draft.
6. **It publishes itself** — at the chosen time the post becomes `published` and is publicly visible.

## 4. Must have and won't do

### Must have
- Schedule a post for a future date/time from the editor
- Timezone-aware: the writer picks in their local zone; the system stores and acts in UTC
- A visible, editable Scheduled state (dashboard + editor), with unschedule and reschedule
- Reliable automatic publish at the chosen time (within ~1 minute)
- Idempotent publishing — a post is never published twice, even if the trigger runs more than once

### Won't do
- Recurring or repeating schedules
- Cross-posting to social or email-on-publish (a later increment)
- "Best time to publish" suggestions
- Scheduling anything other than a whole post (no per-section drip)
- A calendar view of scheduled posts

## 5. How it lands

Surfaced inside the existing editor Publish button — no new top-level navigation, no new page. A writer discovers it the moment they go to publish.

## 6. Rabbit holes

- **Don't build a general job scheduler.** We need one thing — publish this post at this time. A once-a-minute check over due posts is almost certainly enough at our scale; don't reach for queue infrastructure on spec.
- **Timezones.** Store UTC, render local. Decide the writer's timezone source explicitly (profile setting vs browser) and don't let local time leak into storage.

## 7. Open questions

- If the chosen time is already in the past when the writer confirms (clock skew, slow form), do we publish immediately or reject? *(Leaning: reject in the UI, and treat past-time as publish-now on the server as a safety net.)*
- If a scheduled post is edited, does the schedule persist? *(Yes — editing content shouldn't silently unschedule.)*
