---
type: process
last-verified: YYYY-MM-DD
---

# <increment name> — Snag List

**Status:** Open
**Owner:** <name>
**Created:** YYYY-MM-DD
**Related:** [implementation-plan.md](./implementation-plan.md)

Snags found during build/review. Each entry records what was found, where, why it matters, and a
proposed resolution. Capture precisely enough to be actionable later — do NOT inline-fix out-of-scope
issues. Close an entry by moving it to "Resolved" with the commit/PR that fixed it.

## Snags at a glance

| # | Snag | Area | Type | Status |
|---|------|------|------|--------|
| 1 | <short description> | Backend/Frontend/Infra/Full-stack | Architecture/Bug/UX/Feature gap | Open |

---

## Open

### 1. <Snag title>

**Found:** YYYY-MM-DD, <context>.

**Where:** <file/path/area, precise enough to locate>.

**Why it matters:** <impact — link to the rule or contract it violates if applicable>.

**Proposed resolution:** <the fix, or the options if undecided>.

---

## Resolved

### N. <Snag title>

**Found:** YYYY-MM-DD, <context>.
**Resolved:** YYYY-MM-DD, <commit/PR>.

**What it was:** <the problem>.

**Resolution:** <what was done>.
