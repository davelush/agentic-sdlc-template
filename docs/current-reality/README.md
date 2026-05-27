---
type: reference
last-verified: 2026-05-27
---

# current-reality/ — the living truth

How the system, the product, and our way of working actually are **today**. Unlike `future-roadmap/` (uncommitted) and `delivery/` (in flight), nothing here describes plans or history — only current reality. Durable knowledge graduates here from delivery increments on close-out, and is edited directly whenever reality changes outside an increment.

## The areas

- [`practices/`](./practices/playbook.md) — the playbook: how we work with the agent
- [`architecture/`](./architecture/README.md) — how the system works now
- [`decisions/`](./decisions/README.md) — the central ADR log (ADRs graduate here on increment close)
- `development/` — end-to-end how-to recipes (once you add them)

## Anti-rot principles

`current-reality/` docs are only worth keeping if they stay true. These are the rules that keep them from rotting — the hardest-won lesson of maintaining living documentation. They apply across every area above, not just one:

1. **Document what the code can't tell you.** Relationships, invariants, state machines, conventions, and the *why* — the things a search won't reveal. Let the agent derive the rest from the source.

2. **Don't hand-maintain derivable specifics.** Counts ("16 services"), exhaustive file/endpoint/column lists, and per-method signatures rot the fastest and add nothing the agent can't reconstruct on demand. Prefer a conceptual map — areas and their purpose — over an inventory.

3. **Point to ground truth; don't mirror it.** Name the authoritative source — the models and migrations for schema, the generated API spec for endpoints, the router for routes — instead of copying it here, where the copy drifts.

4. **A wrong doc is worse than a missing one.** Especially for anything security-relevant: describe the *real* status, and flag gaps honestly rather than presenting the intended design as if it were live. (See the "Known gaps" section of [architecture/scheduled-publishing.md](./architecture/scheduled-publishing.md) for honest gap-flagging — and resist documenting a control that isn't actually wired in.)

5. **Carry front-matter and keep `last-verified` honest.** Every doc declares `type` and `last-verified`. Update the date whenever you touch the content — a stale `reality` doc is the main failure mode for living documentation.

6. **Verify against the code before certifying.** "Reviewed" means you read the current source, not the last version of the doc. When you revise a doc, re-derive its claims from the tree.

> These principles also explain why the project constitution (`CLAUDE.md`) stays short: it states the binding *rules*; these docs show the *patterns*. Don't duplicate detail across the two — one source of truth each, or they drift apart.
