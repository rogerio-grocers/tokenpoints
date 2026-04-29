# Anti-patterns

Predictable ways to misuse TokenPoints. If you're seeing these in your team, the framework is degrading toward story points with a dollar sign painted on it.

---

## 1. Optimizing cost-per-token instead of cost-per-outcome

**Symptom:** "We saved 30% on our inference bill this quarter." Meanwhile, feature throughput dropped 40%.

**Why it happens:** Cost-per-token is a beautifully clean metric to chase. Cost-per-outcome requires you to know what an outcome is worth, which is harder.

**Fix:** Track outcomes (`merged_clean`, `bug_30d`, etc.) alongside cost. Report them together. Never report cost in isolation to leadership.

---

## 2. Comparing $/task across developers as a performance metric

**Symptom:** "Alice's tasks average $8, Bob's average $22 — Bob needs to be more efficient."

**Why it happens:** Numbers in a column invite comparison. It's irresistible.

**Why it's wrong:** Bob is probably tackling the harder tasks. Or working in a worse-tested area of the codebase. Or using Opus because the work calls for it. Or onboarding a junior. Or any of fifty other reasons.

**Fix:** Hard rule — **TokenPoints is never used in individual performance conversations.** Aggregate at the team level. If managers can't help themselves, remove individual attribution from the data.

This is the rule most likely to be violated. Defend it actively.

---

## 3. Squeezing estimates to fit capacity

**Symptom:** A task is initially estimated as M ($25). Sprint capacity is tight. Estimate magically becomes S ($6) so it fits.

**Why it happens:** Pressure. Same reason story points got inflated and deflated for the last twenty years.

**Why it's worse here:** When the real cost lands at $30 and your estimate said $6, your calibration data is now poisoned. The whole framework relies on honest tracking.

**Fix:** When capacity is tight, the answer is **descope, defer, or accept lower predictability** — not adjust the number. If a stakeholder pressure-tests an estimate, the answer is "here's the data behind the range," not "okay, let's call it smaller."

---

## 4. Estimating tokens directly

**Symptom:** "This task is approximately 250,000 input tokens and 30,000 output tokens."

**Why it happens:** Tokens feel more "scientific" than dollars. They're not.

**Why it's wrong:** Token usage in real agentic sessions is highly variable — same task across different runs can vary 30x. Tokens also don't translate to value at the planning level. Dollars do.

**Fix:** Estimate in dollars, in buckets, ranges. Track tokens for traceability and re-derivation when prices change. Don't put tokens in front of stakeholders.

---

## 5. Ignoring human time

**Symptom:** "This task only cost $4." Meanwhile, three engineers spent six hours each in review and integration.

**Why it happens:** Inference cost is the new shiny number. Human time is the boring old number. People focus on what's interesting.

**Why it's wrong:** A task with $4 of inference and 18 hours of human time is not a $4 task. It's a multi-hundred-dollar task in fully-loaded cost.

**Fix:** Track and report both, always. Never report inference cost without the human-time co-dimension. The ratio of human-time to inference-cost is itself a key calibration signal — if it's drifting upward, your tooling has stopped helping.

---

## 6. One-shot calibration

**Symptom:** "We calibrated last quarter, we're good."

**Why it happens:** Calibration is work. Once-and-done is appealing.

**Why it's wrong:** Model prices change. Your codebase changes. Your team's prompting maturity changes. Calibration isn't a setup step — it's a recurring practice.

**Fix:** Every two sprints, fifteen minutes minimum. Update your team's wiki entry. The day you stop calibrating is the day your numbers start drifting toward fiction.

---

## 7. Velocity-as-trophy

**Symptom:** "Our velocity is now $1,200/sprint, up from $800. We're 50% more productive!"

**Why it happens:** Numbers go up, must be good.

**Why it's wrong:** Inflation is easier than productivity. Higher dollar-velocity could mean more tasks shipped, or more *expensive* tasks shipped (Opus mix shift, larger contexts), or scope inflation, or all three. Without controlling for outcome quality and human time, the number tells you nothing.

**Fix:** Always report velocity alongside outcome distribution and human-time-per-dollar. If all three move together, you have signal. If only the dollar number moves, you have noise (or worse, gaming).

---

## 8. Treating the official scale as gospel

**Symptom:** "The framework says M is $8–$40, but our M tasks always come in at $50. We must be doing it wrong."

**Why it happens:** Scale anxiety. People want a "correct" answer to copy.

**Why it's wrong:** The official scale is a starting anchor, not a benchmark. Your team is doing it right when *your* numbers are honest, not when they match someone else's.

**Fix:** [Calibrate](calibration.md). Update your team's ranges. Stop comparing to the public scale after sprint 2.

---

## 9. Estimating spike work as if it were normal work

**Symptom:** "We have to do this thing nobody on the team understands. Let's call it an L."

**Why it happens:** The scale is right there. Picking a bucket feels like progress.

**Why it's wrong:** When you don't know the shape of the work, *any* estimate is fiction, and committing to it is worse than admitting uncertainty.

**Fix:** Use `??`. Time-box a spike. Produce decomposed sub-tasks. Then estimate.

---

## 10. Ignoring `merged_with_rework` outcomes

**Symptom:** Outcome distribution shows 60% `merged_clean`, 35% `merged_with_rework`, 5% other. Team celebrates the 95% merge rate.

**Why it's wrong:** "Merged with rework" tasks have hidden cost — the rework itself, plus future cognitive overhead from a half-baked initial implementation. They're often more expensive than `reverted` because reverts force a clean redo, while reworks accumulate.

**Fix:** Treat `merged_with_rework` as a yellow flag. If a task type consistently merges-with-rework, the issue is upstream — refinement, decomposition, or test coverage in that area.

---

## 11. Using TokenPoints for non-LLM-augmented work

**Symptom:** Estimating a customer call, an architecture meeting, or an incident response in TokenPoints.

**Why it happens:** Once you have a hammer, etc.

**Why it's wrong:** TokenPoints is calibrated for inference-driven work. Applying it to pure-human work yields meaningless numbers and dilutes your calibration data with noise.

**Fix:** TokenPoints is a tool for one job. Use it for that job, use other tools for other jobs. Hours are still fine for pure-human work.

---

## 12. The "$0.50 myth"

**Symptom:** "Why is my Opus session for the architecture review costing $40? It should be cheap, it's just thinking."

**Why it happens:** People internalized the per-token price of small APIs and didn't update for agentic-coding reality, where context is large, sessions are long, and Opus is 5x.

**Why it's wrong:** A serious agentic session in 2026 routinely hits $20–$80. That's not a bug — that's the cost of doing real engineering work with frontier models.

**Fix:** Re-anchor. Refresh your sense of what "expensive" means. The numbers in [sizing-guide.md](sizing-guide.md) reflect reality, not aspiration.

---

## A meta-pattern: framework theater

The deepest anti-pattern is performing TokenPoints without doing it.

**Symptom:** Estimates filled in dutifully. No tracking. No calibration. No outcome flags. The team uses dollar amounts in planning the same way they used story points — as ritual numbers nobody believes.

**Fix:** If you're not going to track, you're not using TokenPoints. Go back to whatever you were doing before. Tracking is the load-bearing part. Without it, this is just story points with extra steps.
