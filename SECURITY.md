# Security Policy

## Scope

TokenPoints is a documentation and methodology repository. It contains:

- Markdown documentation
- HTML/CSS for the static landing page
- CSV/Markdown templates
- No backend code, no executables, no dependencies that ship to end users

The realistic security surface is small. That said, we take a few things seriously.

## In scope

- **Cross-site scripting (XSS) in the landing page** (`index.html`) — if you find an injection vector in the static site, please report it.
- **Misleading or harmful framework guidance** — if a doc recommends a practice that materially compromises a team's security posture (e.g., suggests storing API keys in tracked CSVs), report it as a security issue, not a regular issue.
- **Dependency confusion or supply-chain risk** — if a future contributor introduces a tool, calculator, or script that pulls compromised dependencies, report it.

## Out of scope

- Issues with how *you* use TokenPoints in your own infrastructure (your tracking sheet exposed credentials, your agent harness leaks tokens, etc.) — those are your responsibility, not ours.
- Theoretical risks without an actionable exploit.
- Broken links, typos, or other non-security bugs — file as a regular issue.

## How to report

**Preferred:** open a [private security advisory](https://github.com/rogerio-grocers/tokenpoints/security/advisories/new) on GitHub. This keeps the report confidential while we triage.

**Alternative:** if you can't use GitHub's advisory feature, contact the maintainer directly via the email listed on their [GitHub profile](https://github.com/rogerio-grocers).

Please include:

- A clear description of the issue
- Steps to reproduce, if applicable
- The specific file, line, or commit involved
- Your assessment of severity (low / medium / high / critical) — we may disagree, but it helps prioritize

## Response timeline

- **Acknowledgment:** within 3 business days
- **Triage and initial assessment:** within 7 days
- **Fix and disclosure timeline:** depends on severity, but we aim to resolve confirmed issues within 30 days

## Disclosure

We follow coordinated disclosure. Once a fix lands, we'll publish a brief security advisory acknowledging the reporter (unless they prefer to remain anonymous).

---

*This policy applies only to the contents of this repository. The framework itself is licensed under [CC BY 4.0](LICENSE) with no warranty.*
