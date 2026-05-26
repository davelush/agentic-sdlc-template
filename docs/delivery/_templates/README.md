---
type: reference
last-verified: 2026-05-26
---

# Delivery templates

Skeletons for the artefacts that make up a delivery increment. Copy the ones you need into a new `delivery/YYYY-mm-dd-<name>/` folder at the start of an increment.

| Template | When | Lives in the increment as |
|----------|------|---------------------------|
| [prd-pitch.md](./prd-pitch.md) | Shape — before anything is planned | `prd-pitch.md` |
| [implementation-plan.md](./implementation-plan.md) | Plan — before any code | `implementation-plan.md` (or `plan.md`) |
| [snag-list.md](./snag-list.md) | Build — when the first out-of-scope issue is found | `snag-list.md` |
| [delivery-review.md](./delivery-review.md) | Review — before the increment closes | `delivery-review.md` |
| [adr.md](./adr.md) | Any time a non-trivial decision is locked | `adr-NNNN-<slug>.md` (graduates to `current-reality/decisions/` on close) |

A typical increment folder ends up looking like:

```
delivery/2026-02-03-scheduled-publishing/
├── prd-pitch.md
├── implementation-plan.md
├── adr-0001-<slug>.md
├── snag-list.md
├── delivery-review.md
└── designs/                    # clickable prototypes, if UI work
```

Not every increment needs every artefact — a pure refactor may have only a plan and a review. For a fully-worked example, see [`delivery/_archive/2026-02-03-scheduled-publishing/`](../_archive/2026-02-03-scheduled-publishing/).
