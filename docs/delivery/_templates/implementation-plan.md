---
type: delivery-plan
status: draft — awaiting sign-off
last-verified: YYYY-MM-DD
---

# <increment name> — Implementation Plan

**Source brief:** [prd-pitch.md](./prd-pitch.md)

The agent writes this plan; the human reviews and signs off before any code is written. It is a live
reference during execution, not filed on day one and forgotten.

## 1. Framing decisions (locked in conversation)
<The decisions already made that the rest of the plan rests on, so they aren't re-litigated mid-build.>

## 2. Where this fits in the current product
<How the increment relates to what already exists — reused surfaces, new entry points, boundaries.>

## 3. User journey, mapped to the system
<Each step of the brief's flow against the screens/endpoints/services that realise it.>

## 4. Data model
<New tables, columns, relationships. Note what is deliberately NOT introduced in this increment.>

## 5. Frontend
<New screens, component reuse, state model.>

## 6. Backend
<New endpoints, services, the layering they follow, permissions.>

## 7. Cross-cutting concerns
<Whatever this increment touches: sharing/visibility, rate limiting, telemetry, caching, jobs.>

## 8. Phasing and ownership
<Phases with explicit gates. Each phase ends with a review-and-sign-off checkpoint. Note coordination
rules that stop people colliding, and any decision that blocks the start.>

### Phase 0 — <name>
- <work>
- **Gate.**

## 9. Risks and rabbit holes
<What could go wrong or balloon, and the mitigation.>

## 10. Deliberate deviations from the brief
<Where the plan intentionally diverges from the pitch, and why.>

## 11. What this plan deliberately does not include
<Explicit out-of-scope list, for traceability against the delivery review.>
