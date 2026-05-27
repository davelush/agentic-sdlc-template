---
type: reference
last-verified: 2026-05-27
---

# docs/ — How this folder is organised

This folder is organised by **lifecycle**, not by topic. A document's *location tells you its state*: whether it describes uncommitted future thinking, work in flight, or how the system works today. Work physically **moves** between these areas as it is shaped, delivered, and completed.

```
future-roadmap/  →  delivery/YYYY-mm-dd-<name>/  →  close-out
   (uncommitted)        (in flight)                  ├─ graduate → current-reality/
                                                      └─ archive  → delivery/_archive/
```

## The four areas

### `future-roadmap/` — uncommitted
What we *might* build. Roadmap, ideas, the parking lot, pitches not yet picked up. Nothing here is a commitment. When an item is picked up for delivery, it moves into `delivery/`.

### `delivery/` — in flight
Work we are actively delivering. One folder per increment, named `YYYY-mm-dd-<name>` where the date is when delivery **started** (it never changes — it sorts increments chronologically and survives archival). Content is heavily templated:
- [`_templates/`](./delivery/_templates/) — skeletons for the pitch, plan, ADR, snag list, and delivery review
- `YYYY-mm-dd-<name>/` — the active increment's artefacts
- `_archive/` — closed increments, kept as the permanent process record

### `current-reality/` — the living truth
How the system, the product, and our way of working actually are **today**: architecture, decisions, practices, and (once you add them) development how-tos. This area is directly editable — not everything here came from an increment.
- `practices/` — the [playbook](./current-reality/practices/playbook.md): how we work with the agent
- `architecture/` — how the system works now
- `decisions/` — the central ADR log (ADRs graduate here on increment close)

### `standing/` — off the lifecycle
Material that doesn't flow through shape → deliver → complete: policies, legal, brand, and similar. It doesn't have a "state" in the lifecycle sense.

## Close-out: graduate, then archive

When an increment completes, it splits in two — do not move the folder wholesale into current reality:

1. **Graduate the durable knowledge.** Fold architecture/standards changes into the existing `current-reality/` docs, and promote any accepted ADRs to `current-reality/decisions/`. Current reality stays *distilled*.
2. **Archive the process record.** Move the remaining pitch, plan, snag list, and delivery review into `delivery/_archive/YYYY-mm-dd-<name>/`. This is the permanent "what we did and why" trail.

See the worked example: the increment archived at [`delivery/_archive/2026-02-03-scheduled-publishing/`](./delivery/_archive/2026-02-03-scheduled-publishing/), with its durable knowledge graduated into [`current-reality/architecture/scheduled-publishing.md`](./current-reality/architecture/scheduled-publishing.md) and [`current-reality/decisions/adr-0001-publishing-trigger.md`](./current-reality/decisions/adr-0001-publishing-trigger.md).

## Front-matter convention

Every document carries front matter so its type and freshness are machine-readable:

```yaml
---
type: reality | process | delivery-plan | near-term | reference
last-verified: YYYY-MM-DD
---
```

- **reality** — how things are now (architecture, runbooks, ADRs, shipped-feature specs)
- **process** — an artefact of how an increment was run (pitch, snag list, delivery review)
- **delivery-plan** — an implementation plan under active execution
- **near-term** — committed-ish roadmap not yet in delivery
- **reference** — indexes, guides, conventions

`last-verified` is the date the content was last confirmed accurate — against the current code, not the previous version of the doc. Stale `reality` docs are the main failure mode for living documentation; see the [anti-rot principles](./current-reality/README.md#anti-rot-principles) for how to keep them honest.
