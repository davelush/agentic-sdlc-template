---
type: reference
last-verified: 2026-05-27
---

# Architecture

How the system works **today**. These are living documents describing current reality — not plans, not history. Durable architectural knowledge graduates here from delivery increments on close-out, and is edited directly whenever reality changes outside an increment.

## The docs

This template ships one example, graduated from the worked increment:

- [scheduled-publishing.md](./scheduled-publishing.md) — how Inkwell's scheduled publishing works (the fictional worked example)

In a real project this folder typically holds a backend walkthrough, a frontend walkthrough, the data model, and a doc per significant subsystem.

## Keeping these docs honest

The [anti-rot principles](../README.md#anti-rot-principles) for living documentation apply here as everywhere under `current-reality/`. The architecture-specific case to watch: don't document a control or component that isn't actually wired in — see the "Known gaps" section of [scheduled-publishing.md](./scheduled-publishing.md) for honest gap-flagging.
