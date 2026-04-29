# The TokenPoints Manifesto

> Story points are dead. Long live TokenPoints.

For two decades, software teams estimated work in story points — an abstract, unfalsifiable unit invented to dodge the obvious failures of hour estimates. It worked, sort of, when humans wrote every line of code.

That world is gone.

In 2026, the first draft of most production code is written by an LLM. The bottleneck is no longer "how long will a developer type this?" It is "how many turns of inference, with which model, against which codebase, will it take to get a working, reviewed, merged change?"

That question has a real answer in dollars. We propose to use it.

---

## The 6 pillars

### 1. Dollars are more honest than hours.

A dollar of inference is a measurable, falsifiable, non-negotiable fact. An hour estimate is a social contract negotiated under pressure. A story point is a vibe in a costume.

When the agent writes the code, the cost of producing that code is no longer a guess — it's a number on the API invoice. Use it.

This does **not** mean human time stops mattering (see pillar 6). It means that for the part of the work the LLM actually does, we now have an honest unit. We should anchor on it.

---

### 2. Variance is information, not noise.

When the real cost of a task is 5x your estimate, that's not "the developer was slow." That is the system telling you the task contained complexity, ambiguity, or codebase friction that you did not see at planning time.

Story points trained teams to *normalize variance away* — to treat divergence from estimate as failure. TokenPoints treats variance as the signal worth investigating. The interesting retro question is no longer *"why did this take longer than we said?"* It's *"what did we learn about this codebase, this prompt, or this model from the gap between estimate and reality?"*

A team whose estimates are perfectly accurate is a team that isn't trying anything new.

---

### 3. Outcome over output.

A developer who routes a hard task to Opus and spends $30 instead of $5 on Sonnet is not being wasteful if the Opus session ships in two turns and the Sonnet session would have looped for fifteen.

Cost-per-token is a metric for the bill. **Cost-per-merged-feature** is a metric for the business. They are not the same number, and confusing them is how you ruin a team.

Optimize the outcome. The bill follows.

---

### 4. Calibrate locally.

This framework ships with a sizing scale. Those numbers are anchors based on observed behavior in 2026 across several teams using current frontier models on midsize codebases.

**They are wrong for you.**

A 100kloc Rails monolith with crisp domain boundaries is not a 4Mloc Java enterprise codebase, and neither resembles a greenfield Next.js project. Your codebase, your model mix, your tooling, your team's prompting maturity — all of these shift the curve.

The scale is a starting point. After two sprints of tracking, you replace our numbers with yours. The methodology is universal; the numbers are local.

---

### 5. Multidimensional, not unidimensional.

The single biggest mistake story points made was collapsing all of "effort" into one number. Teams then optimized that number and broke everything else.

Don't repeat the mistake. Track at minimum:

- **Cost in USD** (the headline)
- **Tokens in / tokens out** (so you can re-derive cost as prices change)
- **Model(s) used** (Opus is ~5x Sonnet on input, this matters)
- **Turns to completion** (a proxy for ambiguity)
- **Human time** (review, planning, integration — see pillar 6)
- **Outcome** (merged? reverted? bugs in prod within 30 days?)

A task that cost $4 and shipped clean is a different animal from a task that cost $4 and got reverted. Don't let one number hide the other.

---

### 6. Human time still exists.

LLMs do not currently do product discovery, stakeholder alignment, code review, deployment orchestration, on-call response, or the dozen other things that turn a working diff into shipped value. Pretending otherwise produces estimates that are dangerously low.

TokenPoints estimates the *inference cost of producing the change.* It does **not** estimate:

- Time spent in planning, refinement, or architecture discussion
- Code review and round-trips with reviewers
- QA, manual testing, staging validation
- Deployment, rollout, monitoring
- Documentation, communication, handoff

Track human time **separately** from inference cost. Both are real. Neither one alone tells you what a task "costs." Anyone selling you a single-number estimate is selling you a story point with a dollar sign painted on it.

---

## What this is not

- **Not a productivity metric for individuals.** Comparing $/task between developers is the modern version of comparing lines of code. It will be gamed, will damage trust, and will measure the wrong thing. Don't do it.
- **Not a way to make estimates "objective."** It is a way to anchor estimates in something measurable. The *forecast* is still a forecast — it's the *retrospective number* that's now honest.
- **Not a replacement for thinking.** A team that adopts TokenPoints and stops asking "is this the right thing to build?" has missed the point entirely.

---

## A small ask

If this framework helps your team, share what you learned. The numbers in the sizing guide get sharper every time another team contributes their calibration. See [CONTRIBUTING.md](CONTRIBUTING.md).

If it doesn't help your team, tell us why. That's more valuable than another success story.

---

*Version 0.1 — open for revision. The pillars are the stable part. The numbers are not.*
