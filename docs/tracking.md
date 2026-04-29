# Tracking

What to measure, how, and why each field matters.

> **Without tracking, calibration cannot happen, and TokenPoints degenerates back into vibes.** This is the non-negotiable step.

---

## The minimum viable schema

Per task, capture:

| Field | Type | Required | Why it matters |
|-------|------|----------|----------------|
| `task_id` | string | yes | Link to ticket / PR |
| `size_estimated` | XS/S/M/L/XL/?? | yes | The bucket you placed it in |
| `cost_estimated_low` | USD | yes | Optimistic |
| `cost_estimated_expected` | USD | yes | Expected |
| `cost_estimated_high` | USD | yes | Pessimistic |
| `model_planned` | string | yes | Sonnet / Opus / Haiku / mixed |
| `human_time_estimated_hours` | float | yes | Refinement + review + QA |
| `cost_real` | USD | yes | Computed from tokens × pricing |
| `tokens_in` | int | yes | Sum across models |
| `tokens_out` | int | yes | Sum across models |
| `model_actual` | string | yes | What you actually used |
| `turns` | int | yes | Number of model turns to completion |
| `human_time_real_hours` | float | yes | Actual human time spent |
| `outcome` | enum | yes | `merged_clean` / `merged_with_rework` / `reverted` / `abandoned` |
| `bug_30d` | bool | filled later | Bug in production within 30 days traced to this task |
| `variance_reason` | string | conditional | Required if real cost is >2x off expected |

The [tracking-sheet.csv](../templates/tracking-sheet.csv) template gives you this schema as a starting spreadsheet.

---

## Field-by-field

### `size_estimated`

The bucket you placed the task in at planning. Don't update it retroactively — even if reality says it was an L, your *estimate* was M, and that gap is the calibration signal. Track real outcomes separately.

### `cost_estimated_*` (three-point)

All three numbers. Don't collapse to one. The spread between optimistic and pessimistic is your *risk premium* for this task — losing it loses information.

### `model_planned` vs. `model_actual`

Often these diverge. You planned Sonnet, escalated to Opus mid-session. That's fine, but track both. Systematic divergence (planned Sonnet, ended up on Opus 60% of the time) is a signal your team's prompting maturity needs work, or your default model is wrong for your codebase.

### `cost_real`

Compute from `tokens × current_pricing`. Don't paste numbers from the API console blindly — pricing changes, and you want a reproducible computation. Most teams just maintain a `pricing.yaml` and a small helper.

### `tokens_in` / `tokens_out`

Capture both. Not just for cost — input tokens correlate with codebase friction (large context = lots of files to load); output tokens correlate with task surface area. They tell you different things.

### `turns`

Number of distinct model interactions to completion. A 50-turn S task is suspicious — either the size was wrong, or the prompting was inefficient, or the codebase area is unusually frictioned. All three are interesting.

### `human_time_real_hours`

Be honest. Include refinement, review round-trips, debugging the agent's output, integration time, deployment shepherding. Exclude unrelated meetings, breaks, and other tasks.

If you can't bring yourself to log human time precisely, log it in 0.5h buckets. Ballpark beats nothing.

### `outcome`

Four states:

- **`merged_clean`** — shipped, no follow-up rework
- **`merged_with_rework`** — shipped, but follow-up commits within a week
- **`reverted`** — got rolled back
- **`abandoned`** — stopped working on it

A task with high cost and `abandoned` outcome is one of the most expensive things on your team. Make these visible — they often point at planning failures, not execution failures.

### `bug_30d`

A retrospective signal. Once a month, look at tasks merged 30+ days ago and flag any that produced production bugs. Tasks that look "cheap" but cause incidents downstream aren't actually cheap.

This field stays empty for ~30 days after a task is merged. That's fine.

### `variance_reason`

Only required when real cost is more than 2x off the expected estimate, in either direction. A short note: "agent looped on test framework version mismatch," "spec was clearer than expected, one-shot," etc.

These notes are gold during calibration meetings. Don't skip them.

---

## What you can skip

- **Per-prompt cost tracking.** Aggregate is fine.
- **Branching agent reasoning paths.** Total tokens is what matters.
- **Prompt provenance.** Useful for prompt engineering, irrelevant for estimation.
- **Real-time dashboards.** A weekly aggregate is enough for calibration.

The temptation to over-instrument is real. Resist it. The minimum schema is the minimum because it's been pruned to what *actually drives calibration decisions.*

---

## Tooling

You don't need anything fancy. Three working setups, in order of overhead:

### Spreadsheet (simplest)

Use the [tracking-sheet.csv](../templates/tracking-sheet.csv) template. One row per task. Update at PR-merge time. Pivot tables for calibration meetings.

This works for teams up to ~10 people.

### Issue tracker fields (medium)

Add custom fields to Jira / Linear / GitHub Issues for the schema above. Pull weekly via API into a simple analytics setup.

### Custom logging (overkill, usually)

Wire your agent harness to log directly to a database. Use only if you're at scale and the spreadsheet is breaking.

---

## Aggregations worth running

In your calibration meeting, look at:

```
SELECT size_estimated,
       COUNT(*) as n,
       MEDIAN(cost_real) as median_cost,
       PERCENTILE(cost_real, 0.25) as p25,
       PERCENTILE(cost_real, 0.75) as p75,
       MEDIAN(cost_real / cost_estimated_expected) as bias_ratio
FROM tasks
WHERE completed_at > NOW() - INTERVAL '4 weeks'
GROUP BY size_estimated;
```

Plus:

- Outcome distribution by size: `% merged_clean / merged_with_rework / reverted / abandoned`
- Total human time / total inference cost (the dimensionality ratio from [calibration.md](calibration.md))
- Top 5 highest-variance tasks (real vs. expected ratio) — discuss in retro

---

## Privacy and reporting

If you're sharing data with the community via [CONTRIBUTING.md](../CONTRIBUTING.md), aggregate before sharing. We're interested in:

- Median costs per bucket
- Variance ratios
- Model mix
- General codebase context (size, language, age)

Not interested in:

- Individual task descriptions
- Per-developer numbers
- Anything proprietary about what you're building

Anonymize accordingly.
