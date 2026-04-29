# Example: Debug session

A worked example of using TokenPoints for an intermittent production bug — including how to handle the fact that "debugging" is fundamentally uncertain.

---

## Task

**Title:** Investigate flaky checkout payment confirmation — sometimes shows "pending" forever.

**Description:** Production bug report from CX. ~3% of checkout flows end up stuck on "payment pending" indefinitely, even though the payment provider confirms success. No clear repro. Started ~2 weeks ago, no obvious deploy correlation.

---

## Refinement

**Gut check:** This is a `??`. We don't know:
- Whether it's a webhook delivery issue, a state machine bug, a database race, or something else
- Whether the fix is one line or a deep redesign
- Whether 3% is the real rate or just what's getting reported

**Decision:** Spike first.

**Spike plan:**
- Time-box: 4h human, $30 inference budget
- Output: a problem statement with root cause identified, plus a sized follow-up task for the fix
- Not in scope: writing the fix itself

---

## Spike execution

### Hour 1 (Sonnet, 6 turns, $4.20)

Loaded relevant logs and the payment confirmation code path into context. Agent identified three candidate hypotheses:

1. Webhook signature validation occasionally rejects valid webhooks
2. Race between webhook handler and frontend polling
3. Idempotency key collision under high load

### Hour 2 (Sonnet, 8 turns, $6.80)

Pulled production logs for the affected sessions. Cross-referenced timestamps. Hypothesis 1 ruled out (no rejected webhook log entries for affected sessions). Hypothesis 3 ruled out (idempotency keys are UUIDs, collision rate is implausible).

Hypothesis 2 looking strong — affected sessions all show the frontend polling endpoint responding "pending" *after* the webhook timestamp by 50-200ms.

### Hour 3 (Opus, 5 turns, $14.00)

Escalated to Opus for the state machine reasoning. Read the session state transition code carefully. Identified the bug: webhook handler updates the payment status in a transaction that doesn't commit until *after* it sends the success response. The frontend polling endpoint reads with `READ COMMITTED` isolation, so for a brief window after webhook receipt, the frontend sees stale "pending" state.

Normally this resolves within milliseconds via the next poll. But the frontend has a "fail open" timeout: if it polls 30 times without seeing "succeeded", it stops polling and shows "pending" permanently. Under load, the gap can occasionally exceed 30 polls.

### Hour 4 (Sonnet, 3 turns, $1.20)

Wrote up the findings. Drafted three follow-up sub-tasks:

| Sub-task | Size | Expected $ |
|----------|------|-----------|
| Fix transaction ordering in webhook handler | S | $5 |
| Increase frontend polling resilience | S | $4 |
| Add monitoring for the "stuck pending" state | S | $6 |

---

## Spike tracking record

| Field | Value |
|-------|-------|
| Type | spike |
| Time-box | 4h human, $30 budget |
| `cost_real` | $26.20 |
| `tokens_in` | 3,800,000 |
| `tokens_out` | 48,000 |
| `model_actual` | sonnet + opus mix (Opus on hour 3 only) |
| `turns` | 22 |
| `human_time_real_hours` | 4.5 |
| `outcome` | spike_complete |
| Output | Root cause identified + 3 sized follow-up tasks |

---

## What this tells us

### What worked

- **Time-boxing the spike.** Without a budget cap, this could have spiraled. The 4h / $30 box forced focus.
- **Mid-session model escalation.** First two hours stayed on Sonnet for hypothesis-narrowing. Hour 3's deep state-machine reasoning was a clear Opus moment. Opus session was 5 turns and decisive — chasing it on Sonnet would have likely taken 15+ turns at lower confidence.
- **Output as decomposed sub-tasks, not code.** The fix doesn't get scoped or estimated until we *know* what the fix is. Spike → understanding → estimation → fix is the correct order.

### What didn't

- **Slightly over time-box** (4.5h vs. 4h, $26 vs. $30 was fine). Pushed into hour 5 to write up findings cleanly. Worth it; rushed write-ups make sub-tasks fuzzier.

---

## The follow-up tasks

The fix tasks are now estimable as plain S tasks. They were executed the next day:

| Sub-task | Estimated $ | Real $ | Outcome |
|----------|------------|--------|---------|
| Fix transaction ordering | $5 | $4.40 | merged_clean |
| Improve frontend polling resilience | $4 | $5.20 | merged_clean |
| Add stuck-pending monitoring | $6 | $7.10 | merged_clean |

**Total fix cost:** $16.70. Spike + fix = $42.90 / 8 hours human.

If we had skipped the spike and just thrown engineers at it, this would likely have been a multi-day, multi-person investigation — easily $200+ and 20+ human hours.

---

## A note on debugging in TokenPoints

**Bugs without a clear repro should always be spikes first.** Forcing them into a sized bucket leads to either:

- Padded estimates (just-in-case budgets that don't reflect reality)
- Catastrophic blow-outs when the bug turns out to be a deep one

The spike-first pattern is the framework's release valve for genuine uncertainty. Use it.

---

## Calibration contribution

- One spike data point: $26.20 / 4.5h with successful root cause identification. Possible team norm: spikes for production bugs typically converge in 4–6 hours and $20–40, when scoped well.
- One Opus-escalation justification: deep state-machine reasoning. Pattern: when narrative reasoning about *invariants* or *order-of-operations* is the bottleneck, escalate. When the bottleneck is just "find and edit," stay on Sonnet.
