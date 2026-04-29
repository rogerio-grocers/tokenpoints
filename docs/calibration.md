# Calibration

How your team turns the baseline scale into *your* numbers.

The official scale is wrong for you. It's wrong for everyone, in a different way. This document is the recipe for making it right.

---

## Why calibration matters

Two teams using the same scale on the same nominal task will produce different real costs because:

- Their **codebases** differ in size, tooling, test quality, and structural clarity.
- Their **model mix** differs (Sonnet-default vs. Opus-heavy).
- Their **prompting maturity** differs (a team using agents for 2 years gets ~3x more done per dollar than one starting today).
- Their **review culture** differs (a team that ships at first-pass uses fewer turns than one that does deep review-driven iteration).

Without calibration, every team is using someone else's numbers. That's worse than no numbers — it's confidently wrong numbers.

---

## The minimum calibration cycle

**Two sprints. Roughly 20 tracked tasks. That's it.**

You don't need fancy analytics. You need consistent tracking and one hour of retrospective every two weeks.

### Sprint 1 — Track without changing anything

Use the [estimation template](../templates/estimation-template.md) and the [tracking sheet](../templates/tracking-sheet.csv) on every task. Estimate using the **baseline ranges** from [sizing-guide.md](sizing-guide.md). Don't try to be accurate — be consistent.

Resist the urge to adjust mid-sprint. You're collecting data; signal needs noise.

### Sprint 2 — Same thing

More data. Now you have ~20 tasks with both estimates and real numbers.

### End of Sprint 2 — The first calibration meeting

One hour. Whole team. You're answering five questions:

---

## The five calibration questions

### 1. What's our median real cost per size bucket?

Pull all your tracked data. For each size (XS, S, M, L), compute the median real cost.

| Size | Baseline range | Our median | Our 25th–75th percentile |
|------|---------------|-----------|--------------------------|
| XS | < $1 | $0.40 | $0.20 – $0.65 |
| S | $1 – $8 | $4.20 | $2.10 – $6.80 |
| M | $8 – $40 | $32 | $18 – $58 |
| L | $40 – $160 | $180 | $95 – $290 |

(Numbers above are illustrative.)

If your medians fall *inside* the baseline ranges, congrats — the framework's anchors mostly work for you, and your action is to tighten the ranges to your interquartile spread.

If your medians fall *above* the top of a range (like the L example), update your team's ranges upward. The next time someone says "this looks like an L," they'll mean *your* L.

### 2. Where is our biggest variance?

For each bucket, compute pessimistic / optimistic ratio of actuals (e.g., 90th percentile divided by 10th percentile). Wide spreads indicate bucket-internal heterogeneity — meaning that bucket is hiding two different kinds of work.

If your L bucket spans $50 to $400 in real outcomes, you don't have one L bucket — you have a "clean L" and a "messy L." Decide whether to split the bucket, change your decomposition triggers, or accept the variance.

### 3. Are we systematically biased?

Compute, per bucket, the ratio of `real cost / expected estimate` (the middle of your three-point estimate).

- Ratio consistently > 1.5 → you're optimistic across the board. Adjust expected estimates upward.
- Ratio consistently < 0.7 → you're pessimistic. Either you're padding, or your codebase is tidier than you think.
- Ratio close to 1.0 with high variance → estimates are roughly right but uncertainty is large; widen pessimistic.

### 4. What does outcome distribution look like?

Pull the outcome flag. What % of tasks per bucket merged clean, vs. needed rework, vs. were reverted?

A bucket with 30% revert rate is telling you something separate from cost — these tasks are *underspecified*, not underestimated. The fix is in your refinement process, not your numbers.

### 5. What's our human-time-to-inference-cost ratio?

Compute total human time (review + planning + QA) divided by total inference cost across the sprint.

| Ratio | Interpretation |
|-------|----------------|
| > 4h per $1 | LLM is barely a bottleneck — most cost is human. Estimate hours, supplement with cost. |
| 0.5–4h per $1 | Healthy range. TokenPoints is your primary unit, hours are a co-dimension. |
| < 0.5h per $1 | Heavily agent-driven. Cost dominates. |

---

## Updating your team's anchors

After the calibration meeting, update your **internal** version of the sizing scale. Keep the official one as a reference for newcomers, but planning runs on yours.

A simple format:

```markdown
## Our team's calibrated sizing (updated 2026-MM-DD)

| Size | Cost (USD) | Notes |
|------|-----------|-------|
| XS | < $0.80 | Slightly tighter than baseline |
| S | $1 – $9 |  |
| M | $9 – $50 | Median lands ~$30 |
| L | $50 – $200 | High variance — consider sub-bucketing |
| XL | $200 – $500 | Mostly decomposed |

Last 20 tasks: median cost $24, median human time 3.2h, model mix 80/20 Sonnet/Opus.
```

Pin this in your team's wiki. Update it every 2 sprints, or sooner if a major shift happens (model price change, major tooling adoption, codebase split).

---

## Common calibration mistakes

**Calibrating after one sprint.** 10 tasks isn't enough — variance dominates. Wait for 2 sprints minimum.

**Adjusting estimates mid-sprint.** This corrupts the data you need to calibrate against. Track first, adjust later.

**Calibrating individuals instead of teams.** Don't. Inter-developer variance in *style* will overwhelm the signal you're after, and you'll end up with politicized numbers. Calibrate the team.

**Re-anchoring on outliers.** One $400 task in your L bucket doesn't mean L is wrong. Look at medians and IQRs, not extremes.

**Stopping calibration once you have numbers.** Model prices change, codebases evolve, team skills mature. Calibration is a continuous loop, not a one-time setup. **Every 2 sprints.**

---

## What "good calibration" looks like

After 2–3 calibration cycles (~6 sprints, ~60 tracked tasks), a healthy team:

- Predicts sprint capacity within ±20%.
- Catches "this task doesn't fit our M bucket — it's secretly an L" earlier in planning.
- Has a shared vocabulary where "this is a $40 thing" is a meaningful claim, not a guess.
- Can tell when the model price changed and re-derive without re-estimating from scratch.

If you're not seeing this after three cycles, the issue is usually inconsistent tracking — not the framework.

---

## Sharing your calibration with the community

Once you have a calibrated team baseline, consider [contributing it back](../CONTRIBUTING.md). Anonymized data from real teams is what turns this framework from one person's idea into a useful empirical reference.
