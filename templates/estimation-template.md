# Estimation Template

Copy this into your ticket / PR description / planning doc. Fill in at refinement time, before work starts.

---

## Task

**ID / link:**
**Title:**
**Brief description:**

---

## Sizing

**Size bucket:** ☐ XS ☐ S ☐ M ☐ L ☐ XL ☐ ?? (spike first)

**If `??`, time-box:** ___ hours human, $___ inference budget. Output is a decomposed plan, not code.

---

## Three-point cost estimate (USD)

| Estimate | Cost | Reasoning |
|----------|------|-----------|
| **Optimistic** | $___ | Everything works, agent one-shots |
| **Expected** | $___ | Realistic, some retry loops |
| **Pessimistic** | $___ | Hits friction, agent loops, re-prompting |

**Risk premium (pessimistic / optimistic ratio):** ___x
*(If >4x, consider splitting or spiking instead of estimating.)*

---

## Model plan

**Default model:** ☐ Haiku-class ☐ Sonnet-class ☐ Opus-class
**Likely escalation:** ☐ Yes (to Opus) ☐ No

**Reasoning:** _Why this model? (e.g., "well-tested area, Sonnet handles it" or "novel architectural reasoning, start in Opus")_

---

## Human time estimate

| Activity | Hours |
|----------|-------|
| Refinement & planning |  |
| Active session (review, prompt iteration) |  |
| Code review |  |
| QA / manual validation |  |
| Deployment / monitoring |  |
| **Total** |  |

---

## Anchor

**Closest past task:** _(link)_
**That task's real cost:** $___ / ___ hours human
**Adjustments from anchor:**
- ☐ Larger surface area (+)
- ☐ Smaller surface area (−)
- ☐ More ambiguous requirements (+)
- ☐ Cleaner requirements (−)
- ☐ Worse-tested area (+)
- ☐ Better-tested area (−)
- ☐ Other: _______

---

## Risks / unknowns

_(Anything that could push toward pessimistic. Be specific.)_

-
-
-

---

## Decomposition (if applicable)

_(Required for XL. Recommended for L. Optional for M.)_

| Sub-task | Size | Expected $ |
|----------|------|-----------|
|  |  |  |
|  |  |  |
|  |  |  |

---

## Tracking (filled in at completion)

| Field | Value |
|-------|-------|
| `cost_real` | $___ |
| `tokens_in` | ___ |
| `tokens_out` | ___ |
| `model_actual` | ___ |
| `turns` | ___ |
| `human_time_real_hours` | ___ |
| `outcome` | ☐ merged_clean ☐ merged_with_rework ☐ reverted ☐ abandoned |
| `variance_reason` *(if real >2x or <0.5x of expected)* | ___ |
| `bug_30d` *(filled in 30 days post-merge)* | ☐ Yes ☐ No |
