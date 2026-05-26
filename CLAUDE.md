# CLAUDE.md — Project Constitution (template)

> **This is a template.** Replace the *Project Overview* and *Conventions* sections with your own product and stack. The **Agentic Working Agreement**, the **docs-lifecycle wiring**, and the **Key reference documents** section are the transferable core — keep them, lightly adapted. Delete this banner once you've made it yours.

This file is read automatically by the agent at the start of every session. It defines how we work and the conventions for changing this codebase. Treat it as binding: these instructions override default behaviour.

## Project Overview

<One short paragraph: what the product is and who it's for.>

Keep the `docs/` block in the structure below — it wires the agent into the lifecycle documentation. Replace the rest with your real layout.

```
your-project/
├── ...               # your application code
└── docs/             # Documentation, organised by lifecycle (see docs/README.md)
    ├── future-roadmap/   # Uncommitted: roadmap, ideas, future specs
    ├── delivery/         # In-flight increments (templated artefacts)
    ├── current-reality/  # How the system, product, and process work today
    └── standing/         # Off-lifecycle: policies, etc.
```

## Agentic Working Agreement

These rules are stack-independent. They are the method — keep them. See [docs/current-reality/practices/playbook.md](docs/current-reality/practices/playbook.md) for the reasoning behind each.

- **Plan before implementing.** Start every piece of work by reading the task and proposing an approach in writing. Nothing is built without a stated approach the human has reviewed. For anything larger than a small change, write the plan into `docs/delivery/YYYY-mm-dd-<name>/` from the [templates](docs/delivery/_templates/).
- **Options, then pick.** For non-trivial decisions, present numbered options with a recommendation; the human chooses. Generate the design space — don't make the call for them. Keep it conversational for deep work; don't force nuance into a terse multiple-choice widget.
- **Verify locally before opening a PR.** Hard rule: no PR without local review first. Prompt for this step; never skip it.
- **Capture, don't inline-fix.** When you notice something out of scope, add it to the increment's snag list — don't fix it inline. Unscoped inline fixes are the biggest source of scope drift. Name it, defer it, keep going.
- **Corrections are registered once.** When the human corrects you, change the behaviour immediately and durably. Don't make them repeat it.
- **No time estimates.** Don't write "~30 minutes" or "quick change". They're noise and they anchor badly.
- **Investigate, then decide — never investigate-and-accidentally-mutate.** When you have access to production (logs, database, infrastructure), reads are free; writes require an explicit decision. Default to read-only.
- **Keep current reality distilled.** When an increment closes, graduate its durable knowledge into `current-reality/` and archive the process record. Don't let `current-reality/` become a graveyard of finished increments.

## Conventions (replace with your own)

> The block below is an **illustrative example** of the *shape* of good conventions — specific, enforceable, and explaining the *why*. Replace it entirely with the real rules for your stack. (This example happens to describe a layered backend; yours might be about component structure, module boundaries, state management, error handling, naming — whatever your codebase actually needs to stay coherent.)

**Example — layered architecture.** All backend code follows a strict layering, and each layer talks only to the one directly below it:

```
Endpoint (HTTP) → Service (business logic) → Repository (data access) → Model (ORM)
```

- Endpoints handle HTTP and validation; they call services, never repositories.
- Services hold business logic; they never reach into another layer's internals.
- Repositories own data access and the database session.
- No reach-through between non-adjacent layers.

The point of writing this down is that the agent (and humans) will otherwise drift toward shortcuts — and a rule that lives only in someone's head can't be pointed at in review.

## Testing

<How tests are structured and how to run them. Be concrete — exact commands. Example:>

- Unit tests: `<command>` — mock dependencies; one behaviour per test.
- Integration/component tests: `<command>` — real database, real HTTP.

## Common mistakes to avoid

<A short, battle-earned list of the traps specific to *your* codebase. Keep it tight — these are the things that actually keep going wrong, not a style guide. Example:>

1. Forgetting to scope a query to the current tenant/user.
2. Skipping a layer for "just this one quick call".
3. Editing generated files by hand instead of regenerating them.

## Key reference documents

`docs/` is organised by lifecycle — see [docs/README.md](docs/README.md) for the map. The stable references live under `docs/current-reality/`:

- [docs/current-reality/practices/playbook.md](docs/current-reality/practices/playbook.md) — how we work with the agent
- `docs/current-reality/architecture/` — how the system works today
- `docs/current-reality/decisions/` — Architecture Decision Records: the settled choices; don't reopen one without a superseding ADR

## How-to guides

<Once you've written end-to-end recipes (e.g. "adding an endpoint", "adding a screen"), list them here under `docs/current-reality/development/`. They encode the conventions that aren't visible from grep alone — consult them before writing new code in that area.>
