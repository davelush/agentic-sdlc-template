---
type: reality
last-verified: 2026-05-26
---

# Agentic Squad SDLC — A Playbook

**Status:** Working draft
**Context:** Distilled from real practice on a production SaaS product, then genericised. Intended as a living reference, not a finished methodology.

This is an opinionated description of how a small squad should work when an AI coding agent is a first-class member of the team. It is not a framework. It is a set of practices that have been tested and found to work, alongside a list of problems that haven't been fully solved yet.

---

## The two loops

Everything in this model operates at one of two levels.

**The product loop** governs what gets built and whether it was the right thing. It runs over days and weeks.

**The execution loop** governs how individual pieces of work land. It runs over minutes and hours.

They are nested. You cannot collapse them into one without losing either strategic discipline or execution rhythm. The goal is to make both loops fast and lightweight — not to eliminate one of them.

---

## The product loop

```
Shape → Plan → Build → Review
```

### 1. Shape

Write a pitch before touching implementation. It scopes a single meaningful product increment — enough to feel like progress, small enough to close in a cycle.

Shape Up language is useful here: **appetite** (how much is this worth?), **must-haves** (what makes this increment real?), and a clear **won't-do list** (scope discipline is a first-class deliverable, not a constraint).

The pitch is the document you review the delivery against. If it isn't written clearly enough to do that, it isn't ready. (Template: [`prd-pitch.md`](../../delivery/_templates/prd-pitch.md).)

### 2. Plan

Convert the pitch into a phased implementation plan in a markdown doc before any code is written. The plan answers:

- What are the phases and their gates?
- What is the data model?
- What are the deliberate deviations from the brief and why?
- What coordination rules prevent people from conflicting?

The agent writes the plan; the human reviews and signs off. The plan is a live reference during execution, not a document filed on day one and never reopened. (Template: [`implementation-plan.md`](../../delivery/_templates/implementation-plan.md).)

### 3. Build

Work through the plan phase by phase. Each phase ends with a gate: review what was built, mark decisions as closed, explicitly agree to start the next phase. Phases should be small enough that the gate doesn't feel like a ceremony.

Snags found during build go on a live snag list in the same folder as the plan — not into a backlog somewhere else, and not fixed inline if they're out of scope. Capture them precisely enough to be actionable. (Template: [`snag-list.md`](../../delivery/_templates/snag-list.md).)

### 4. Review

Write a delivery review before the cycle closes. It answers three questions:

1. What did we actually deliver against the brief?
2. Where did we fall short?
3. Where did we deviate or add beyond scope?

The delivery review is written by the agent, reading git history and the source tree against the original pitch. It is quick to produce and creates a permanent record of what the cycle produced and what it didn't. (Template: [`delivery-review.md`](../../delivery/_templates/delivery-review.md).)

**Stakeholder comms happen at this level**, not per-ticket. Group a handful of tickets into a coherent delivery, ship them, and share one demo of the working increment — not a ticket-by-ticket update.

---

## The execution loop

```
Task → Plan/options → Pick → Implement → Verify → PR
```

### Open with the task

Start every piece of work by reading the task (the tracker card, the issue). The agent reads it and proposes an approach. This is the plan-before-implement discipline: nothing is built without a stated approach the human has reviewed.

### Options, then pick

The agent presents numbered options for non-trivial decisions; the human selects. This pattern — the agent generates the option set, the human makes the call — operates at every level: architecture, UX, copy, infrastructure.

The key property: the human stays in the decision loop without having to generate the options themselves. This is a genuine division of labour.

**Avoid the multiple-choice widget for deep work.** A structured choice format is fine for shallow decisions but too concise for ones that need discussion. During implementation-heavy sessions, keep decisions conversational: the agent states a direction, the human confirms or corrects in plain text.

### Implement and verify locally

Implementation follows from the stated approach. Before any PR is opened, the human verifies locally. This is a hard rule: no PR without local review first. The agent should prompt for this step, not skip it.

### Close the task

Tick the checklist on the task — written in detail before implementation starts, checked against at close. Update the PR title and description. Move the card.

### Design note

For UI work, the execution loop has an extra step between plan and implement: **a clickable prototype** (plain HTML is enough). Run it in two passes:

1. **Journey-level first.** Skeleton screens that establish the flow and key states — no detailed styling. The goal is a design light enough to review at the structural level; a fully-formed concept makes it hard to see and challenge the journey underneath the detail.
2. **Detail pass.** Once the journey is stable, revisit each step in high fidelity, reviewed on a shareable preview URL.

The mistake to avoid: arriving at review with a finished-looking design when the journey itself is still unresolved. Detail commands attention; it does not invite structural critique.

---

## Parallelisation

The agentic method raises individual throughput significantly. The instinct is to parallelise more work across more people. The evidence suggests this is the wrong instinct.

**Smaller squads move faster.** More people means more hand-offs, more coordination overhead, and more context that has to be shared before anything can proceed. One or two people on a tightly scoped increment outperform a larger team on the same scope.

**The right parallelisation unit is the product experiment, not the person.** Rather than adding people to one stream, run independent product experiments in parallel — each owned by a small squad with its own pitch, its own column, and its own delivery review.

**Independence is the hard constraint.** Parallel streams that touch the same data model, the same endpoints, or the same shared components will collide. If you cannot draw a clean line between streams, they are not independent and should be sequenced, not parallelised.

---

## Working with the agent

A few practices that have proven durable:

**Plan documents are written before implementation starts.** The agent writes, the human reviews. Writing the plan is the mechanism by which misunderstandings surface before they cost anything.

**Corrections are delivered directly and registered permanently.** When the agent does something wrong — opens a PR without review, adds time estimates, breaks a documented convention — the correction is blunt and the behaviour changes immediately. Don't soften corrections and don't repeat them. If you're repeating a correction, something else is wrong.

**Production is part of the conversation.** Log inspection, database queries, infrastructure — these can happen inside sessions, not in a separate terminal. The discipline to go with this: investigate-then-decide, never investigate-and-accidentally-mutate.

**Gap docs and snag lists prevent scope inflation.** When something out of scope is noticed during a session, it goes onto the snag list immediately. The instinct to inline-fix every issue is the single biggest source of scope drift. Name the issue, defer it, keep going.

**This works part-time.** The throughput doesn't come from hours. It comes from the method — a high decision-per-minute rate, the elimination of context-switching, and implementation delegated to the agent. A focused hour a day is enough to sustain meaningful delivery velocity.

---

## Open questions and unsolved problems

These are the things that don't have clean answers yet.

**How do you coordinate two people on the same codebase at the same time?** One person with an agent is well understood. Two people, each with their own session, working in the same repo simultaneously — the conflict patterns are not. Worktrees? Strict branch discipline? Explicit per-session file ownership? This needs to be tested and written down.

**How do you prevent the constitution's rules from drifting in practice?** A documented rule can still be violated — the rule was there, it just wasn't followed. Catching it in review and adding it to the snag list is reactive. Is there a faster feedback loop — a pre-commit check, a test that fails on the violation?

**When does the snag list get worked?** Snags accumulate during delivery and sit on a list. The delivery review names them. But there's often no defined moment when they get fixed — neither scheduled into the current cycle nor explicitly deferred. This creates a growing inventory of acknowledged debt with no owner or deadline.

**How do you onboard someone new into this workflow?** The method carries a lot of implicit knowledge — the options pattern, the plan-first discipline, the local-review rule. This doc tries to make it explicit, but a newcomer still has to internalise it through practice. What is the minimum viable pairing session that transfers it?

**How much is transferable across stacks?** The loops and the decision pattern should transfer cleanly. The specific tool integrations won't. The constitution's conventions are stack-specific by definition.

**What happens when the agent is wrong about architecture?** The pattern assumes the agent generates options and the human selects. This works when the human has enough context to evaluate them. When they don't — new to the codebase or the domain — a bad option can get picked. Plan-first and local-review are partial mitigations; the deeper question is knowing when to trust the agent's option set and when to go find an external reference.

**How do you handle a session that runs too long?** Very long sessions accumulate enough context that later decisions are made without full awareness of earlier ones. There's no clean mechanism yet for compacting mid-session while preserving the important state — partly a tooling question, partly a working-practice one about when to close a session and open a fresh one with a structured handoff.
