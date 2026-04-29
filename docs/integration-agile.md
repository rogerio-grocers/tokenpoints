# Integrating TokenPoints with Agile workflows

You don't have to throw out Scrum, Kanban, or whatever you currently run. TokenPoints replaces the *unit of estimation*, not the ceremonies around it.

This guide covers the mechanics of dropping it in.

---

## Scrum

### Backlog refinement

Replace the "story points" column with **size + three-point cost estimate + human-time estimate.**

```
| Title                         | Size | $ low | $ exp | $ high | Human h | Model    |
|-------------------------------|------|-------|-------|--------|---------|----------|
| Add CSV export to admin panel | M    | $12   | $25   | $60    | 4h      | Sonnet   |
| Fix flaky integration test    | S    | $2    | $5    | $12    | 2h      | Sonnet   |
| Refactor session middleware   | L    | $50   | $110  | $240   | 12h     | Opus mix |
```

Refinement still happens — it just lands on cost ranges instead of point values. Anchor on past similar tasks (see [framework.md](framework.md)).

### Sprint planning

Instead of "we have 40 points of capacity," you have:

- **Inference budget**: e.g., $800 for the sprint
- **Human-time budget**: e.g., 200h across the team

Pull tasks until *expected cost* hits ~80% of the inference budget AND *expected human time* hits ~80% of the time budget. Whichever you saturate first is your binding constraint.

> **Why 80%, not 100%?** Variance is real. Pessimistic costs are typically 2–3x the expected. Reserve 20% for the long tail. If you fill to 100%, you'll over-commit on most sprints.

### Daily standup

No change. Talking about what you did, what you'll do, blockers — that doesn't depend on the estimation unit.

Optional addition: when reporting "I'm working on X," note current burn (e.g., "I'm on the auth refactor, $35 in, going better than expected"). Helps surface tasks that are blowing past their range early.

### Sprint review

Show what you shipped. Stakeholder-facing — they don't need to see cost per task unless cost is a stakeholder concern.

### Sprint retrospective

This is where TokenPoints adds the most value over story points. Three new questions to ask:

1. **Which tasks blew past their pessimistic estimate, and why?** (variance is information)
2. **Which tasks came in well under optimistic, and why?** (also information)
3. **What's our outcome distribution this sprint?** (clean merges vs. rework vs. reverts)

Every two sprints, add the [calibration meeting](calibration.md) onto the back of retro.

### Velocity

**Velocity = sum of real cost of merged tasks per sprint.** Plus, as a co-dimension, total human time delivered.

This number is honest in a way story-point velocity never was. If your velocity went from $600/sprint to $400/sprint, *something* changed — model price, team composition, codebase friction, scope inflation. You can investigate. Story-point velocity drops were folklore by comparison.

---

## Kanban

### WIP limits

You probably already have WIP limits by count. Consider adding:

- **WIP limit by inference budget**: no more than $X of in-flight work at once
- **WIP limit by L+ tasks**: no more than 1–2 large tasks in flight per team

L tasks especially benefit from sequential focus — an L blocked behind an L behind an L is how cycle time blows up.

### Cycle time

Track cost-per-merged-task and human-time-per-merged-task as Kanban metrics alongside cycle time. Three metrics give you a much richer picture than cycle time alone.

### Replenishment

When deciding what to pull next, prefer tasks that:

1. Have well-bounded cost ranges (high optimistic-to-pessimistic ratio = uncertainty = consider spiking instead)
2. Don't push WIP-by-budget past your limit
3. Are sized to your team's calibrated comfort zone

---

## SAFe / scaled frameworks

If you're in SAFe, LeSS, or similar:

- **PI planning** uses costs at the feature level. Bottom-up: features decompose into stories with TokenPoints estimates; sum gives you a feature-level cost range.
- **Capacity** at PI level is the sum of team capacities (inference budget + human time).
- **Cross-team dependencies** still hurt. TokenPoints doesn't fix dependency hell — it just lets you describe it in dollars.

The framework scales because dollars compose additively. Story points famously do not (a 5-pointer on team A is not a 5-pointer on team B). One real advantage.

---

## Working with non-engineering stakeholders

### Product

Product doesn't need to learn the framework. They need to know:

- "How big is this?" → answered by the size bucket name (M, L, etc.)
- "Can we do it this sprint?" → answered by capacity math, same as before
- "Is it worth it?" → cost is now a *visible* input to ROI conversations

The big shift: when product asks "how much would it cost to add X feature," you can give them a real range in dollars. Not "5 points," not "two weeks." Dollars. They will love this.

### Engineering management

Three new things they can do:

1. **Compare cost-per-merged-feature across teams** with caveats — only meaningful when calibrated and similar codebases.
2. **Forecast inference budget** for the next quarter based on roadmap × past calibration data.
3. **Identify outlier tasks** where cost is wildly disproportionate to outcome — usually a planning or scope issue.

What they should NOT do:

1. **Compare cost-per-task across individuals.** It's the modern lines-of-code. Hard rule.
2. **Squeeze estimates to fit budget.** This is the failure mode that killed story points and will kill TokenPoints if allowed.

### Finance

Finance is the one stakeholder group that benefits the most from this framework. For the first time, they can:

- See actual cost-of-engineering at the feature level
- Forecast LLM spend with real basis
- Calculate true ROI per feature

Treat them as an ally. They will help you defend the framework against the bad behaviors above.

---

## Migration path

### Week 1–2: shadow mode

Estimate in story points (or whatever) **and** in TokenPoints. Track real costs. Don't change planning.

### Week 3–4: dual mode

Start using TokenPoints in planning meetings. Keep story points as a comparison column. Watch for resistance and confusion — note where the new system feels rough.

### Week 5–6: TokenPoints primary

Drop the legacy unit. Run a calibration meeting at the end of week 6.

### Week 7+: refine

Iterate on your team's calibrated ranges. Update your wiki entry every two sprints.

If after 6 weeks the framework isn't sticking, *don't* force it. Either the team isn't doing enough agent-driven work for it to matter, or the framework needs adaptation for your context. Both are fine outcomes — file an issue if you have feedback.

---

## What stays the same

- Refinement, planning, retro, review ceremonies
- The role of product manager, EM, etc.
- Definition of done
- Code review standards
- Deployment processes
- Anything that wasn't about the unit of estimation

What changes is the unit. The discipline around it gets *easier*, because the unit is now grounded in something falsifiable.
