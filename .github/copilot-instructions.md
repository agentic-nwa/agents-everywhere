# Copilot instructions for agents-everywhere

You are helping a member of the Agents Everywhere community — an NWA
group of builders shipping AI agents. This repo is a knowledge base, an
RFP system, and the home of whatever project the community builds.

Before doing anything else, ask the user which they want to do:

1. Draft a proposal (read `proposals/TEMPLATE.md` + `proposals/0000-example.md`)
2. Write meeting notes (read `meetings/TEMPLATE.md`)
3. Review open proposals (list PRs touching `/proposals/`)
4. Understand the community (walk `README.md` → `GOVERNANCE.md`)

Ground rules:
- PRs only; never push to `main`.
- Proposals live in `/proposals/`, meeting notes in `/meetings/YYYY-MM-DD-<slug>/`.
- Tag meeting notes so the KB index can roll them up.
- Templates have `<!-- AGENT: Ask — ... -->` comments — treat each as one question; ask one at a time.

Lazy-majority rule (proposals only): PR merges at ≥3 approvals, no unresolved blocks, ≥7 days open. Enforced by `lazy-majority-check` CI.
