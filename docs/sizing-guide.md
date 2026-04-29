# Sizing Guide

The TokenPoints scale, with worked examples for each bucket.

> **Reminder:** these numbers are **starting anchors**, not laws. Your team will recalibrate. See [calibration.md](calibration.md).

---

## The scale at a glance

| Size | Cost (USD) | Turns | Human time | Decompose? |
|------|-----------|-------|------------|------------|
| **XS** | < $1 | 1–4 | < 30 min | No |
| **S** | $1 – $8 | 5–15 | 30 min – 2h | No |
| **M** | $8 – $40 | 15–40 | 2 – 8h | Optional |
| **L** | $40 – $160 | 40–100 | 1 – 3 days | Recommended |
| **XL** | $160 – $400 | 100+ | 3+ days | **Required** |
| **??** | unknown | n/a | time-boxed | Spike first |

"Turns" assumes a Sonnet-class model. Opus sessions hit the same outcome in fewer turns at higher per-turn cost. Haiku flips the ratio.

---

## XS — < $1

**The pattern:** a focused, well-scoped change to a small surface area. The agent likely produces correct output in 1–3 attempts. You spend more time reviewing than the agent spends generating.

**Examples:**
- Rename a function and update its callers within one module.
- Fix a typo in a user-facing string and update the related test.
- Add a missing null check that a linter or PR comment flagged.
- Bump a dependency version with no API changes and verify the lockfile.

**Red flags this is actually bigger:**
- "Rename a function" but it's a public API used in 200 places.
- "Fix this bug" but the root cause is unclear.

**Tracking note:** XS tasks are easy to skip tracking on. Don't. They aggregate, and the median XS cost is one of the most useful calibration numbers you'll have.

---

## S — $1 – $8

**The pattern:** a single-file or tightly-scoped multi-file change. The agent does 5–15 turns of work, including some back-and-forth on edge cases or test coverage. Human time goes mostly to review and merge prep.

**Examples:**
- Add a new endpoint that follows an existing pattern in the codebase (CRUD-style).
- Implement a new validation rule in a form, with tests.
- Add structured logging to a service following the existing logging conventions.
- Fix a bug with a clear repro and a localized fix.
- Add a feature flag and gate an existing behavior behind it.

**Distinguishing from XS:** S has either nontrivial logic, a meaningful test suite update, or 2–3 files touched. XS is *one obvious change.*

**Distinguishing from M:** S follows an existing pattern. The moment the agent has to *invent* the pattern (because nothing similar exists in the codebase), you're in M territory.

---

## M — $8 – $40

**The pattern:** a feature spanning several files, requiring some cross-cutting reasoning. The agent will likely loop on test failures, edge cases, or unclear requirements. Mid-session, you may refine the prompt or hand-edit to unblock.

**Examples:**
- Implement a new domain feature (e.g., "users can schedule a recurring reminder") in a codebase that doesn't already have scheduling primitives.
- Migrate a service from one library version to another with breaking changes (small surface area).
- Add a non-trivial UI component with state management, API integration, and tests.
- Implement a webhook receiver including signature validation, persistence, and retry logic.
- Fix a bug whose root cause spans 2–3 modules.

**Red flags this is actually L:**
- The feature touches 5+ files across unrelated modules.
- It requires modifying a shared abstraction other features depend on.
- The agent has gone 30+ turns without converging.

**Decomposition is optional but often helpful** — splitting an M into two S's gives you tighter feedback and cleaner PRs.

---

## L — $40 – $160

**The pattern:** substantive engineering work. Refactoring, deep debugging, cross-module features, library migrations with real surface area. The agent will need significant guidance, often multiple sessions, and human-driven architectural decisions in between.

**Examples:**
- Refactor a tangled module into cleaner abstractions while preserving behavior.
- Track down a flaky test across an async pipeline, including the actual fix.
- Migrate an authentication system from session cookies to JWT (or vice versa).
- Replace one ORM with another in a single bounded context.
- Add a meaningful new capability to a shared utility used across the codebase.

**Decomposition is recommended.** L tasks have wide variance. Splitting into 2–3 M tasks usually yields better predictability and faster review cycles.

**Multi-session reality:** L work typically spans multiple agent sessions. Track each session and aggregate. A session that ends with "I made progress but it's not done" still consumed real money — log it.

---

## XL — $160 – $400

**The pattern:** architectural changes, multi-system features, large refactors, or migrations that genuinely touch the spine of the codebase. The agent is one tool among several; significant human design and review drives the work.

**Examples:**
- Extract a service from a monolith.
- Replace a database engine in a single bounded context (Postgres → DynamoDB for the events table).
- Implement a new top-level domain feature that spans frontend, backend, and infrastructure.
- Significant security-sensitive rework (e.g., adding multi-tenancy to a previously single-tenant app).

**Decomposition is required.** Do not run an XL task as a single unit of planning. Split it into a sequence of L's and M's, ship them incrementally, and track each.

If you find yourself unable to decompose, **the task is `??`, not XL.** Spike first.

---

## ?? — Spike first

**The pattern:** you don't know enough yet to estimate honestly. Forcing a number is fiction.

**When to use:**
- A new area of the codebase nobody has touched recently.
- A feature with unclear requirements.
- A bug with no reproduction path.
- A "this might be easy or this might be a nightmare" gut check.

**How to handle:**
1. Time-box the spike (e.g., 4 hours human + $25 inference budget).
2. Output is **not** code — it's a decomposed plan with M/L sized sub-tasks and identified risks.
3. After the spike, the work becomes one or more concrete sized tasks.

A spike that produces a working prototype is still a spike — its goal was *learning*, not shipping. The real implementation gets re-estimated based on what you learned.

---

## Modifiers

The base size is just the starting point. Apply modifiers when relevant:

### Model premium

| Model class | Multiplier on cost |
|-------------|-------------------|
| Haiku-class | ~0.2x |
| Sonnet-class | 1x (baseline) |
| Opus-class | ~5x |
| Mixed (Sonnet → Opus escalation) | ~2x (rough blend) |

### Codebase friction

Soft adjustments based on the area being touched:

- **Well-tested, well-structured area:** -25%
- **Legacy area, sparse tests, weird abstractions:** +50% to +100%
- **Generated code, obscure DSL, or domain the model has weak training on:** +100%+

### Team prompting maturity

A team that has been using agents for two years will systematically estimate lower (and be right) than a team that started last month doing the same work. Calibration absorbs this — don't try to model it explicitly.

---

## A worked example

**Task:** "Add SSO support via Okta to our admin panel."

**First reaction:** sounds like an L? Maybe XL?

**Decomposition:**

1. Spike: research Okta integration patterns in our stack (`??` → 4h, $20 budget)
2. Add Okta SDK dependency, basic config plumbing (S, ~$5)
3. Implement OAuth callback endpoint with token validation (M, ~$25)
4. Wire admin panel login flow to Okta with feature flag (M, ~$20)
5. Add tests for callback handling and session creation (S, ~$8)
6. Documentation update (XS, ~$1)

**Total estimate:** ~$80 inference + ~16h human time, spread across 5 tracked tasks.

Notice how decomposition turned an L/XL fog into five concretely-sized tasks. That's the point.

---

## What about non-coding agent work?

This guide is calibrated to coding tasks. If you're using LLMs for non-coding work (research, content, classification), the same logic applies but the cost curves differ. We don't have great anchors for those domains yet — if you have data, [contribute it](../CONTRIBUTING.md).
