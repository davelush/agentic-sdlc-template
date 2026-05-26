---
type: process
last-verified: YYYY-MM-DD
---

# <increment name> — Delivery Review

**Status:** Review
**Created:** YYYY-MM-DD
**Source brief:** [prd-pitch.md](./prd-pitch.md)
**Implementation plan:** [implementation-plan.md](./implementation-plan.md)

This document assesses what was actually shipped against the original brief, based on git history
(commits `<first>` → `<last>`) and the source tree. Written by the agent reading the commits and the
spec. It is organised as three questions.

---

## 1. How we achieved the brief
<What was delivered, end to end. Walk the brief's must-haves and core flow against the real screens,
endpoints, and tables that realise them. Link to the code.>

## 2. Where we fell short
<Must-haves missed, corners cut, known gaps. Be honest — this is the permanent record. Cross-reference
the snag list for anything captured but not yet closed.>

## 3. Where we deviated from the brief and added beyond it

### Deliberate deviations (decided in the implementation plan)
<Where the build intentionally diverged, and why.>

### Additions not in the brief
<Anything shipped beyond the original scope.>

## Close-out
<What graduates into `current-reality/` (architecture, ADRs) and what is archived. State the open-snag
position at close.>
