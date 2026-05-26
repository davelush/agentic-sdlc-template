# Agentic SDLC Template

A reusable template for running a software project where an AI coding agent (e.g. [Claude Code](https://claude.com/claude-code)) is a first-class member of a small team.

It gives you two things that work together:

1. **A documentation structure organised by lifecycle** — a document's *location* tells you its state: uncommitted future thinking, work in flight, or how the system works today. Work physically *moves* as it is shaped, delivered, and completed.
2. **A way of working** — a method [playbook](docs/current-reality/practices/playbook.md), a project constitution ([CLAUDE.md](CLAUDE.md)), and [delivery templates](docs/delivery/_templates/) that turn the method into something you can actually run.

It is distilled from real practice on a production SaaS product and then genericised. It is opinionated, deliberately small, and not a framework.

## The core idea: location is state

Most docs folders are organised by topic, so nothing tells you whether a document describes a settled reality, a commitment, or a daydream. Here the **lifecycle is the primary axis**:

```
future-roadmap/  →  delivery/YYYY-mm-dd-<name>/  →  close-out
   (uncommitted)        (in flight)                  ├─ graduate → current-reality/
                                                      └─ archive  → delivery/_archive/
```

A few principles fall out of this:

- **Current reality stays distilled.** When an increment finishes, its *durable knowledge* graduates into `current-reality/` and its *process record* (pitch, plan, snags, review) is archived — you never dump a finished increment wholesale into the living docs.
- **The agent plans before it implements.** Nothing is built without a written approach the human has reviewed.
- **The human decides; the agent generates the options.** A genuine division of labour.
- **Capture-and-defer beats inline-fixing.** Out-of-scope discoveries go on a snag list, not into the current change.

## What's in here

```
.
├── CLAUDE.md                     # project constitution (template — make it yours)
└── docs/
    ├── README.md                 # the lifecycle map + conventions
    ├── future-roadmap/           # uncommitted: roadmap, ideas, parking lot
    ├── delivery/
    │   ├── _templates/           # PRD pitch, plan, ADR, snag list, delivery review
    │   └── _archive/             # closed increments (process record)
    ├── current-reality/
    │   ├── practices/playbook.md # how a squad works with the agent
    │   ├── architecture/         # how the system works today
    │   └── decisions/            # ADR log — settled choices
    └── standing/                 # off-lifecycle: policies, etc.
```

## How to use it

1. Copy `docs/` and `CLAUDE.md` into your repo.
2. Rewrite `CLAUDE.md`'s *Project Overview* and *Conventions* for your product and stack. **Keep** the *Working Agreement* and the docs-lifecycle wiring — that's the transferable core.
3. Read [the playbook](docs/current-reality/practices/playbook.md).
4. Start your first increment by copying the [templates](docs/delivery/_templates/) into `docs/delivery/YYYY-mm-dd-<your-feature>/`.

## The worked example

To make it concrete, this repo includes a complete worked increment for a **fictional** product — **Inkwell**, a lightweight blogging platform — shipping a **scheduled publishing** feature, run end to end:

- The pitch, plan, snag list, and delivery review in [`docs/delivery/_archive/2026-02-03-scheduled-publishing/`](docs/delivery/_archive/2026-02-03-scheduled-publishing/)
- Its durable knowledge graduated into [`current-reality/architecture/scheduled-publishing.md`](docs/current-reality/architecture/scheduled-publishing.md) and [`current-reality/decisions/adr-0001-publishing-trigger.md`](docs/current-reality/decisions/adr-0001-publishing-trigger.md)

Everything about Inkwell is invented — it exists only to show the artefacts in use.

## Tooling

The method is tool-agnostic. The examples mention a task tracker, a chat-based coding agent, and pull-request review — substitute your own (Linear/Jira/Basecamp, GitHub/GitLab, and so on). The lifecycle and the decision pattern transfer; the specific integrations don't.

## Licence

Apache-2.0. Derived from real practice — adapt it freely, and keep what earns its place.
