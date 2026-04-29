# Example: Frontend feature

A worked example of estimating, executing, and tracking a typical frontend feature with TokenPoints.

---

## Task

**Title:** Add a CSV export button to the admin user list page.

**Description:** The admin panel has a paginated user list. Add a button that exports the currently filtered set of users to CSV. Should respect existing filters and search query. Include columns: id, email, signup_date, last_login, status.

---

## Refinement

**Anchor:** Three months ago, we added a similar export to the orders list. That task cost $14 real, took 3.5 hours human, was a clean merge. Sonnet-only.

**Adjustments from anchor:**
- (+) New columns require a small data shape transformation (orders export was a 1:1 dump)
- (−) Better-tested area now (we added testing infra in Q1)
- (~) Same complexity otherwise

**Conclusion:** Slightly bigger than the orders task, but not meaningfully. Estimating ~$15–20 expected.

---

## Sizing decision

**Size: S**
**Three-point estimate:**
- Optimistic: $8
- Expected: $18
- Pessimistic: $40

**Risk premium:** 5x — slightly high. Risk is mainly around the data transformation step (which columns, how to format dates, whether to include deleted users). Could spike, but the spec is clear enough — just flag during review.

**Model plan:** Sonnet-default. No expected escalation.

**Human time estimate:**
- Refinement: 0.5h
- Active session: 1h
- Code review: 1h
- QA: 0.5h
- **Total: 3h**

---

## Execution

**Session 1** (Sonnet, 8 turns):
- Agent reads the existing orders export for pattern reference
- Generates the export endpoint and frontend button
- First-pass tests fail because dates are returned as ISO strings; CSV expects formatted dates
- Refines, all tests pass

**Session 2** (Sonnet, 4 turns):
- Code review feedback: "deleted users shouldn't appear in admin export by default"
- Agent adds filter and a test case

---

## Tracking record

| Field | Value |
|-------|-------|
| `size_estimated` | S |
| `cost_estimated_low` | $8 |
| `cost_estimated_expected` | $18 |
| `cost_estimated_high` | $40 |
| `cost_real` | $14.20 |
| `tokens_in` | 980,000 |
| `tokens_out` | 21,000 |
| `model_planned` | sonnet |
| `model_actual` | sonnet |
| `turns` | 12 |
| `human_time_estimated_hours` | 3 |
| `human_time_real_hours` | 2.75 |
| `outcome` | merged_clean |
| `variance_reason` | n/a (within range) |
| `bug_30d` | (filled in 30 days post-merge) |

---

## What this tells us

- **Estimate was accurate.** Real cost ($14.20) landed near the optimistic-to-expected midpoint. Anchoring against the orders export worked.
- **Human time was slightly under.** Refinement was tight because the spec was clear; review was fast because patterns matched.
- **Model planning was correct.** No escalation needed. Sonnet handled it.

**Calibration contribution:** Adds another data point in the S bucket median ($14 here, vs. our team's running median of $4.20). This task was on the high end of S — close to M territory. Worth flagging in calibration meeting whether tasks like this should anchor as "small M" instead.
