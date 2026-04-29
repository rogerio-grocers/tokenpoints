# Contributing

TokenPoints gets sharper every time another team contributes. Three types of contribution are most valuable:

---

## 1. Calibration data (most valuable)

Anonymized data from teams using the framework is what turns this from one person's idea into an empirical reference.

**To contribute calibration data, open an issue using the "Share Calibration" template** with:

- Team size (no individual data)
- Codebase context (language, rough size, age)
- Model mix (% Sonnet / Opus / Haiku / other)
- Median real cost per size bucket (XS through XL)
- Interquartile range per bucket if available
- Outcome distribution (% merged_clean etc.)
- How long you've been calibrating (number of sprints / tasks)

We do **not** want:
- Individual task descriptions
- Per-developer numbers
- Anything that would identify your project or codebase

The calibration data goes into `data/community-calibrations/` (created when the first contribution arrives) and feeds an aggregated public reference.

---

## 2. Framework feedback

Open an issue if:

- A pillar in the manifesto doesn't match your team's experience.
- A bucket range is consistently wrong for your context (and you can describe the context).
- An anti-pattern in `docs/anti-patterns.md` happened to you in a way the doc doesn't describe.
- You found a different anti-pattern worth adding.

Strong opinions welcome. The framework is v0.1; revisions are expected.

---

## 3. Documentation improvements

Pull requests welcome for:

- Typos, broken links, formatting fixes
- Worked examples in additional domains (data engineering, ML training, content workflows)
- Translation of the README into other languages (start with `README.<lang>.md`)
- Better diagrams (the ASCII art in `docs/framework.md` is functional, not beautiful)

For substantive changes (rewriting a doc, adding a new doc), open an issue first to discuss.

---

## What not to PR (without discussion first)

- Changes to the 6 pillars in the manifesto
- New buckets in the sizing scale (XS–XL is intentionally bounded)
- Removal of `human_time` tracking (this is a non-negotiable counterweight)
- "Productivity scoring" features

These are all defensible discussions, but they need conversation in an issue before code lands.

---

## Issue templates

The repo has issue templates for:

- **Share Calibration** — for contributing your team's data
- **Sizing Feedback** — when buckets feel wrong for your context
- **Anti-pattern Report** — a misuse you saw in the wild
- **General Discussion** — anything else

---

## Code of conduct

Be useful, be honest, be specific. Drive-by opinions without context are fine but get less attention than data-backed feedback. Disagreement is welcome. Personal attacks are not.

---

## License

By contributing, you agree your contribution is licensed under [CC BY 4.0](LICENSE), the same as the rest of the repo.
