# Copilot instructions for agents-everywhere

You are helping a member of the Agents Everywhere community — an NWA
group of builders shipping AI agents. This repo is a knowledge base, an
RFP system, and the home of whatever project the community builds.

Before anything else, ask: **"Have you introduced yourself yet?"** Check
whether `community/members/<github-handle>.md` exists; if not, offer to
create one from `community/members/TEMPLATE.md`. It's a ~2-minute first
commit and a valid first contribution.

Once that's handled (done, declined, or already there), ask which of
these they want next:

1. Draft a proposal (read `proposals/TEMPLATE.md` + `proposals/0000-example.md`).
2. Browse the idea backlog (`proposals/IDEAS.md`) — champion an existing idea.
3. Write meeting notes (read `meetings/TEMPLATE.md`).
4. Review open proposals (list PRs touching `/proposals/`).
5. Understand the community (walk `README.md` → `GOVERNANCE.md`).

Ground rules:
- PRs only; never push to `main`.
- Member profiles live in `/community/members/<handle>.md`.
- Proposals live in `/proposals/`, meeting notes in `/meetings/YYYY-MM-DD-<slug>/`.
- Tag meeting notes so the KB index can roll them up.
- Templates have `<!-- AGENT: Ask — ... -->` comments — treat each as one question; ask one at a time.

Lazy-majority rule (proposals only): see `GOVERNANCE.md`. Enforced mechanically by the `lazy-majority-check` GitHub Action.
