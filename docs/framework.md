# The TokenPoints Framework

End-to-end overview. If you've read the [README](../README.md) and [Manifesto](../MANIFESTO.md), this is the operational layer underneath.

---

## What problem this solves

Software estimation has always been bad. In the LLM era, it got worse for a new reason: **a developer routing a task to Opus and a developer routing the same task to Sonnet produce wildly different cost curves**, and neither maps cleanly to "hours" or "story points."

Meanwhile, the data exists. Every API call returns a token count. Every token count converts to dollars. The information is sitting there, in the invoice, ignored.

TokenPoints is a vocabulary and process for actually using it.

---

## The estimation loop

```
                         ┌─────────────────────┐
                         │   1. Decompose      │
                         │   (XL+ → split)     │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   2. Anchor         │
                         │   (find similar     │
                         │   past task)        │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   3. Three-point    │
                         │   (optimistic /     │
                         │   expected /        │
                         │   pessimistic)      │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   4. Model premium  │
                         │   (Opus ≈ 5× Sonnet │
                         │   on input)         │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   5. Human overhead │
                         │   (review, QA,      │
                         │   planning — track  │
                         │   separately)       │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   6. Execute &      │
                         │   track (real cost, │
                         │   tokens, model,    │
                         │   turns, outcome)   │
                         └─────────┬───────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │   7. Recalibrate    │
                         │   (every 2 sprints) │
                         └─────────────────────┘
```

---

## Step 1 — Decompose

If your gut says "this is XL or bigger," **decompose before estimating.** TokenPoints is not designed for tasks that take more than 3 days of human time or burn more than ~$400 of inference. The variance at that scale is too wide to plan against.

If you cannot decompose because you don't know the shape of the work yet, that task is **`??`** — a spike. Time-box the investigation (e.g., 4 hours, $20 budget), produce decomposed sub-tasks, then estimate those.

> **Rule of thumb:** if three engineers in the room give you sizes spanning three buckets (e.g., M / L / XL), it's a spike. Don't average — investigate.

---

## Step 2 — Anchor

Find the closest task your team already shipped. That task has a *real* cost number attached to it. Use it as the reference, then adjust:

- Bigger surface area → bump up
- More ambiguous requirements → bump up
- Cleaner codebase touched → bump down
- Better-tested area → bump down

This is identical to how planning poker is supposed to work, except the anchor is a dollar amount with a paper trail, not a folkloric "remember when we did the auth thing?"

If you have **no past task to anchor against**, you are calibrating from scratch — see [calibration.md](calibration.md). Use the baseline scale and expect 2–3x variance until you have ~20 tracked tasks.

---

## Step 3 — Three-point estimate

For each task, produce three numbers:

| Estimate | Meaning |
|----------|---------|
| **Optimistic** | Everything works, agent one-shots most of it |
| **Expected** | Realistic — some retry loops, some context refinement |
| **Pessimistic** | Hits codebase friction, agent loops, requires re-prompting |

The **gap between optimistic and pessimistic is your risk premium**, made explicit. If the gap is more than 4x, the task is too uncertain — consider splitting or spiking.

Don't average the three to get one number. Carry the range into planning. A sprint of "expected $200, pessimistic $600" tells you something an aggregated $300 hides.

---

## Step 4 — Model premium

Pricing changes constantly. As of 2026, rough multipliers vs. mid-tier Sonnet:

| Model class | Input multiplier | Output multiplier | Use when |
|-------------|------------------|-------------------|----------|
| Haiku-class | ~0.2x | ~0.2x | Simple, narrow, deterministic edits |
| Sonnet-class | 1x | 1x | Default for almost everything |
| Opus-class | ~5x | ~5x | Architectural reasoning, novel problems, deep debugging |

If you plan to route a task to Opus, multiply your Sonnet-anchored estimate accordingly. If you will likely *escalate* mid-task (start Sonnet, hand to Opus on stuck), estimate as a blend.

**Verify current pricing at estimation time.** The multipliers above will be wrong by the time you read this.

---

## Step 5 — Human overhead

Estimate human time **separately** from inference cost. Don't merge them. The minimum to track:

- Refinement & planning (before the agent runs)
- Code review (you, the reviewer, whoever)
- QA / manual validation
- Deployment & monitoring

A task with $5 of inference and 4 hours of human review is not a "$5 task." Be honest about both numbers and report both.

---

## Step 6 — Execute & track

Run the work. Capture:

- **Tokens in / tokens out** (per model, if you used multiple)
- **Real USD cost** (computed from tokens × current pricing)
- **Turns to completion**
- **Human time spent**
- **Outcome flag** — merged clean / merged with rework / reverted / abandoned
- **30-day flag** (filled in later) — bugs in prod traced to this task

The [tracking template](../templates/tracking-sheet.csv) gives you the schema.

> **Without tracking, calibration cannot happen, and TokenPoints degenerates back into vibes.** This is the non-negotiable step.

---

## Step 7 — Recalibrate

Every two sprints, look at your tracked data and ask:

1. What's our median actual cost per size bucket? Does it match the baseline ranges?
2. Where's our biggest variance? Which size bucket is least predictable?
3. Are we systematically over- or under-estimating in one direction?
4. What outcome ratio (clean merge vs. revert) are we seeing per size?

Update *your team's* anchors. The official scale stays as a reference for newcomers, but your planning runs on your numbers. See [calibration.md](calibration.md) for the mechanics.

---

## Sprint planning with TokenPoints

**Velocity** = sum of *real* USD cost of tasks completed in a sprint.

**Capacity** = budget cap for next sprint, set by leadership or by the team's own historical median.

**Planning** = pull tasks until estimated *expected* cost (not pessimistic, not optimistic) hits ~80% of capacity. Reserve 20% for variance and unplanned work.

**Anti-pattern alert:** if pressure mounts to "fit more into the sprint," do **not** shrink estimates to make tasks fit. That is the modern version of story-point inflation, and it kills the framework. Instead, descope, defer, or accept lower predictability — but keep the numbers honest. See [anti-patterns.md](anti-patterns.md).

---

## What this looks like in practice

**Before TokenPoints:**

> *"This auth refactor is a 5. Maybe an 8. Let's call it 5."*
> *(Sprint ends. It took 3x as long. Team adjusts velocity downward. No one knows why.)*

**With TokenPoints:**

> *"Auth refactor. Closest analog: the session-handling rework last quarter, which cost $87 real and 12 hours human. This one touches more files but the area is better tested now. Optimistic $60 / Expected $110 / Pessimistic $220, plus ~10 hours human. Sonnet-default with possible Opus escalation if the JWT logic gets weird."*
>
> *(Sprint ends. Real cost: $145 / 14h human. Variance flagged: JWT did get weird, escalated to Opus mid-session. Lesson logged. Next similar task estimated with Opus blend baked in.)*

The second version is more work to produce. It is also dramatically more useful, and the data compounds.

---

## When NOT to use TokenPoints

- **Pure-human work.** If a task is 100% human (architecture review, customer interviews, incident postmortems), use whatever you used before. TokenPoints has nothing to add.
- **Pre-production exploration.** During greenfield exploration where you don't know what you're building, estimating is theater regardless of unit. Use rough budgets ("this exploration gets a $200 cap") instead of estimates.
- **Teams without LLM tooling adopted.** If your team isn't actually using agents to write code, this framework solves a problem you don't have.

---

## Next steps

- Read the **[Sizing Guide](sizing-guide.md)** for worked examples of each bucket.
- Pull the **[estimation template](../templates/estimation-template.md)** into your workflow.
- After 10 tracked tasks, run **[calibration](calibration.md)**.
