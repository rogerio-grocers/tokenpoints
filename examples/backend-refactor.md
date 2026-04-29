# Example: Backend refactor

A worked example of a larger task that decomposes — and what happens when reality blows past pessimistic.

---

## Task

**Title:** Refactor session management to support multi-tenant scoping.

**Description:** Our session middleware currently assumes single-tenant. We're rolling out multi-tenancy in Q3. Sessions need to carry a `tenant_id`, and all session lookups need to scope by it. Touches the auth middleware, session store, several route handlers, and some shared utility modules.

---

## Refinement

**Initial gut check:** This sounds like an L. Maybe XL.

**Decomposition decision:** L+, definitely decompose. After 30 minutes of investigation:

| Sub-task | Size | Expected $ | Human hours |
|----------|------|-----------|-------------|
| 1. Add `tenant_id` to session schema, run migration | S | $6 | 2 |
| 2. Update session creation flow to populate `tenant_id` | M | $20 | 4 |
| 3. Update session lookup to scope by tenant | M | $25 | 4 |
| 4. Update route handlers that touch sessions | M | $30 | 6 |
| 5. Update tests across all touched areas | M | $15 | 3 |
| 6. Backwards-compatibility shim for existing sessions during rollout | S | $7 | 2 |
| **Total** |  | **$103** | **21** |

**Rolled-up three-point estimate (computed from per-task ranges):**
- Optimistic: $55
- Expected: $103
- Pessimistic: $260

**Model plan:** Sonnet for sub-tasks 1, 5, 6. Sonnet with possible Opus escalation for 2, 3, 4 (the auth-touching ones). Reasoning: auth code has subtle invariants worth Opus's deeper reasoning when something's off.

---

## Execution — what actually happened

### Sub-task 1: schema migration (estimated $6, real $4.80)
Clean. Sonnet, 4 turns, merged in 1.5h.

### Sub-task 2: session creation (estimated $20, real $42)
**Variance: 2.1x.** What happened: agent's first implementation broke the OAuth callback flow because it assumed `tenant_id` was always available at session creation, but for OAuth callbacks the tenant has to be derived from the OAuth state parameter. Required Opus escalation to debug. 28 turns total.

**Variance reason logged:** "OAuth callback path required tenant resolution from OAuth state, not assumed in initial spec."

### Sub-task 3: session lookup (estimated $25, real $22)
Came in slightly under. Sonnet handled it cleanly — the patterns were now established by sub-task 2.

### Sub-task 4: route handlers (estimated $30, real $38)
Slightly over. 14 handlers needed updating. Agent did most of it cleanly; 3 handlers had unusual session-dependency patterns that needed manual edits.

### Sub-task 5: tests (estimated $15, real $25)
**Variance: 1.7x.** Test fixtures didn't have `tenant_id` plumbing. Updating fixtures cascaded across more test files than expected.

### Sub-task 6: backwards-compatibility shim (estimated $7, real $6)
Clean.

---

## Tracking record (rolled up)

| Field | Estimated | Real |
|-------|-----------|------|
| Total cost | $103 expected ($55–$260 range) | **$137.80** |
| Total human hours | 21 | 26 |
| Outcome | — | merged_with_rework (1 minor patch after rollout) |
| `bug_30d` | — | false (no production bugs in 30 days) |

**Within range?** Yes. Real cost ($137.80) lands between expected ($103) and pessimistic ($260). The decomposition saved us — without it, this would have been a single L estimated at maybe $80, and we'd have blown past pessimistic by 70%.

---

## What this tells us

### What worked

- **Decomposition.** Six tracked sub-tasks instead of one foggy L gave us early signal (sub-task 2's blow-up was visible at sub-task 2, not at the end of week 3).
- **Three-point estimate.** Pessimistic of $260 absorbed the variance honestly. Nobody had to lie to leadership.
- **Calibration data.** Five new data points across S/M buckets, plus one variance event with a captured reason.

### What didn't

- **Sub-task 2's spec missed the OAuth callback flow.** Refinement question for next time: "what session-creation paths exist beyond the obvious one?"
- **Test fixture cost was hidden inside sub-task 5.** Next time, "test fixture refactor" might deserve its own sub-task when the migration crosses many test files.

---

## Calibration contribution

Adds:
- Two M data points landing meaningfully above bucket median ($42, $38 vs. team median of ~$30) — these were the auth-touching ones. Possible insight: "M tasks in the auth module run ~30% higher than M tasks elsewhere." File for next calibration meeting.
- One variance reason linked to a refinement gap (OAuth state). Pattern-spot at retro: are we missing alternate code paths during refinement systematically?

---

## What this would have looked like without TokenPoints

> "Auth refactor is a 13-pointer. Maybe an 8?"
>
> *(Sprint ends. It took 3 weeks. Team is exhausted. Velocity drops next sprint as a defensive reaction.)*

Compare with TokenPoints: real cost $138, 26 human hours, with documented variance reasons and clear calibration takeaways. Same work, dramatically more learning extracted.
