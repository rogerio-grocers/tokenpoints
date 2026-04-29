# TokenPoints

> **Story points are dead. Long live TokenPoints.**

A framework for estimating software work in **dollars of LLM inference**, not hours or story points.

---

## The shift

When humans wrote 100% of the code, hours were a (bad) proxy for effort. Story points tried to fix this and mostly made it weirder — abstract, unfalsifiable, and trivially gameable.

In 2026, when an agent writes the first draft of most of your code, the question changed:

> **"How much will this task cost to ship?"**

That cost is now a *measurable, falsifiable number in USD* — the price of the tokens the model burns getting the work done. Tracking it is no harder than tracking time, and unlike time, it doesn't lie.

TokenPoints is a planning vocabulary built around that number.

---

## What you get

- **A 6-pillar manifesto** — why dollars beat points, and where the limits are.
- **A sizing scale (XS → XL)** — calibrated to real agentic-coding sessions in 2026, anchored in USD ranges, not vibes.
- **A calibration playbook** — how your team turns the scale into *your* numbers in two sprints.
- **Tracking templates** — the minimum data to capture so calibration actually happens.
- **Agile integration guides** — how this drops into Scrum, Kanban, or whatever you already run.
- **Anti-patterns** — the obvious ways to misuse this and how to avoid them.

---

## Quick start

1. Read the **[Manifesto](MANIFESTO.md)** (5 min). If you disagree, this framework isn't for you and that's fine.
2. Skim the **[Sizing Guide](docs/sizing-guide.md)** to internalize the XS–XL scale.
3. Use the **[estimation template](templates/estimation-template.md)** on your next 10 tasks. Don't change anything else yet.
4. After two sprints, run **[calibration](docs/calibration.md)** with your real data.
5. Once calibrated, integrate into planning — **[integration guide](docs/integration-agile.md)**.

> **Time to first useful estimate: ~10 minutes.**
> **Time to a calibrated team baseline: ~2 sprints.**

---

## The sizing scale (uncalibrated baseline)

| Size | Cost (USD) | Typical pattern | Human time |
|------|-----------|-----------------|------------|
| **XS** | < $1 | Pinpoint edit, autocomplete-heavy | < 30 min |
| **S** | $1 – $8 | Single-file feature/bug, 5–15 turns | 30 min – 2h |
| **M** | $8 – $40 | Multi-file feature, 15–40 turns | 2 – 8h |
| **L** | $40 – $160 | Refactor, deep debug, cross-module | 1 – 3 days |
| **XL** | $160 – $400 | Architectural change, multi-system | 3+ days |
| **??** | unknown | Spike first — investigate before sizing | time-boxed |

Anything above XL **must be decomposed.** If you can't decompose it, you don't understand it yet — that's a spike.

These ranges are **starting anchors**, not laws. Your team will diverge based on codebase size, model mix, and tooling. See **[Calibration](docs/calibration.md)**.

---

## The 6 pillars

1. **Dollars are more honest than hours.**
2. **Variance is information, not noise.**
3. **Outcome over output.**
4. **Calibrate locally.**
5. **Multidimensional, not unidimensional.**
6. **Human time still exists.**

Full elaboration: **[MANIFESTO.md](MANIFESTO.md)**.

---

## Repository map

```
tokenpoints/
├── README.md                       ← you are here
├── MANIFESTO.md                    ← the 6 pillars
├── docs/
│   ├── framework.md                ← end-to-end overview
│   ├── sizing-guide.md             ← XS–XL with worked examples
│   ├── calibration.md              ← turn the scale into your numbers
│   ├── tracking.md                 ← what to measure, how
│   ├── integration-agile.md        ← Scrum / Kanban fit
│   └── anti-patterns.md            ← how to misuse this
├── templates/
│   ├── estimation-template.md
│   ├── tracking-sheet.csv
│   └── retrospective-template.md
├── examples/
│   ├── frontend-feature.md
│   ├── backend-refactor.md
│   └── debug-session.md
└── CONTRIBUTING.md
```

---

## Contributing

The most valuable thing you can contribute is **your team's calibration data** — anonymized averages, model mix, codebase context. Over time, this turns the repo from one person's framework into an empirical reference.

See **[CONTRIBUTING.md](CONTRIBUTING.md)** for how.

---

## Status

**v0.1 — early draft.** Names, ranges, and pillars are open for community input. If you have a strong opinion, open an issue or a PR. The scale will evolve as more teams report data.

---

## License

[CC BY 4.0](LICENSE) — use it, fork it, build on it. Just credit the source.

---

*Created by [Rogério](https://github.com/rogerio-grocers) — proposal, not prescription.*
