# Design: agentic-nwa/agents-everywhere as a community OSS repo

**Date:** 2026-04-18
**Status:** Approved (awaiting implementation plan)
**Authors:** Jordan Carlisle (+ pairing agent)

## Purpose

Turn `agentic-nwa/agents-everywhere` into a single repo that plays three
roles for the NWA Agents Everywhere community:

1. **Knowledge base** — a durable record of what gets shared at each meeting.
2. **RFP system** — a lightweight way for members to propose what the
   community should build together.
3. **Home of the community project** — where the accepted proposal is
   implemented (in-repo or spun out).

The repo must be **agent-optimized**: a new member should be able to clone
the repo, point it at Claude Code / Cursor / Codex / OpenClaw, and get
walked through the community and the contribution flow without reading
any docs first. Optimizing for agents also tightens the repo for humans.

## Non-goals

- A polished marketing site for the event — the Luma page + blast emails
  handle that. This repo is evergreen and builder-facing.
- Heavy governance. We use lazy majority and volunteer maintainers, not
  a steering committee.
- Event logistics (dates, fees, Luma links). Those live in per-event
  artifacts, not in the repo.

## Decisions (from brainstorming)

| Decision | Choice | Rationale |
|---|---|---|
| RFP mechanism | Hybrid: Discussions for ideation → PR against `/proposals/` for formal RFPs | Low friction at idea stage, reviewable doc at proposal stage |
| Winning project location | Per-proposal (in-repo or new-repo) | Let the shape fit the project, not the reverse |
| Acceptance rule | Lazy majority: ≥3 approvals + 7-day block window | Fast, lightweight, async-friendly |
| KB organization | Per-meeting folders + frontmatter tags + auto-generated tag rollup | Natural unit of creation (meeting) + topic-based discovery |
| Maintainer model | Volunteer, rotating, self-declared | Anyone active can opt in; inactive rotate off after 3 months |
| Org | `agentic-nwa/agents-everywhere` (repo transferred 2026-04-18) | Obvious community home; makes spun-out project repos trivial |

## Architecture

### Repo layout

```
agents-everywhere/
├── README.md                   # Community intro + agent onboarding +
│                                 tech stack + references + volunteers
├── AGENTS.md                   # Universal agent entrypoint
├── CLAUDE.md                   # Claude Code shim → "see AGENTS.md"
├── CONTRIBUTING.md             # How to propose / write notes / vote
├── GOVERNANCE.md               # Lazy majority, maintainers, branch rules
├── CODE_OF_CONDUCT.md          # Contributor Covenant 2.1
├── LICENSE                     # Existing
│
├── proposals/
│   ├── TEMPLATE.md             # Agent-walkable RFP template
│   └── 0000-example.md         # "Shared Agent Eval Harness" worked example
│
├── meetings/
│   ├── TEMPLATE.md             # Agent-walkable meeting-notes template
│   └── 2026-04-18-kickoff/
│       └── README.md           # Seed entry
│
├── knowledge-base/
│   └── index.md                # Auto-generated tag rollup (CI-managed)
│
├── .claude/
│   └── skills/
│       ├── draft-proposal/SKILL.md
│       ├── write-meeting-notes/SKILL.md
│       └── review-proposals/SKILL.md
│
├── .cursor/
│   └── rules/
│       └── agents-everywhere.mdc
│
├── .github/
│   ├── CODEOWNERS
│   ├── copilot-instructions.md
│   ├── DISCUSSION_TEMPLATE/
│   │   └── idea.yml
│   ├── ISSUE_TEMPLATE/
│   │   ├── proposal-followup.yml
│   │   ├── meeting-note.yml
│   │   └── bug.yml
│   ├── PULL_REQUEST_TEMPLATE/
│   │   ├── proposal.md
│   │   └── default.md
│   └── workflows/
│       ├── lazy-majority.yml
│       └── kb-index.yml
│
└── docs/
    └── specs/
        └── 2026-04-18-oss-repo-design.md   # This file
```

### Three flows, one repo

**Flow 1 — Propose**

1. Member has an idea. Posts in `Discussions → Ideas` (optional, low friction).
2. When an idea has traction, member (or their agent via
   `.claude/skills/draft-proposal/`) creates `proposals/YYYY-MM-DD-<slug>.md`
   from `TEMPLATE.md` and opens a PR.
3. `lazy-majority-check` Action runs on the PR; becomes a required status
   check once it determines the PR touches `/proposals/**`.
4. PR merges when: ≥3 approvals, 0 unresolved blocking reviews, PR age ≥7
   days. Enforced mechanically by the Action + branch protection.
5. On merge, a maintainer opens a `proposal-followup` Issue linked to the
   merged proposal, tracking implementation (in-repo or in the spun-out
   repo).

**Flow 2 — Document a meeting**

1. Member (or their agent via `.claude/skills/write-meeting-notes/`)
   creates `meetings/YYYY-MM-DD-<slug>/README.md` from `TEMPLATE.md`.
2. Fills in frontmatter (date, title, tags, attendees).
3. Drops slides/demos in sibling `slides/` and `demos/` folders.
4. Opens a PR. Single maintainer approval is enough to merge (no lazy
   majority — meeting notes are record, not governance).
5. `kb-index.yml` workflow rebuilds `knowledge-base/index.md` on merge.

**Flow 3 — Vote / review**

1. Member opens the PR list, filters `proposals/`.
2. Or: asks their agent to run `review-proposals`, which summarizes open
   proposals and walks through voting.
3. Member leaves a 👍 approval or a blocking review with a stated reason.
4. `lazy-majority-check` status updates on each review event.

### Agent-readable layer

Every flow has a corresponding agent entrypoint so no flow requires
reading docs first:

- `AGENTS.md` — top-level router. Tells the agent to ask the user which
  flow they're doing, then dispatch to the matching skill or template.
- `.claude/skills/{draft-proposal,write-meeting-notes,review-proposals}/SKILL.md`
  — concrete multi-step flows for Claude Code.
- `.cursor/rules/agents-everywhere.mdc` — same content, Cursor convention.
- `.github/copilot-instructions.md` — same content, Copilot convention.
- `CLAUDE.md` — one-line shim pointing at `AGENTS.md` to avoid drift.

Templates (`proposals/TEMPLATE.md`, `meetings/TEMPLATE.md`) are
**agent-interview-shaped**: each section has an `<!-- AGENT: Ask — ... -->`
HTML comment naming exactly one question. The agent asks questions in
order, fills answers in, reads back, revises. Humans filling them in
manually get the same prompts inline as comments.

### Governance (lazy majority)

- Baseline branch protection: require PR, require 1 approval, require
  conversation resolution, block force pushes, block deletions.
- `/proposals/**` PRs additionally require the `lazy-majority-check`
  status check, which enforces ≥3 approvals, 0 unresolved blocks, and
  PR age ≥7 days. All three conditions must hold for the check to pass.
- Maintainers are listed in `.github/CODEOWNERS`. Anyone can become one
  by opening an Issue and not being blocked. Anyone inactive 3 months
  rotates off. No extra vote weight — maintainers execute the merge,
  they don't decide.
- Changes to `GOVERNANCE.md` itself are proposals subject to lazy
  majority (i.e., they land via a PR that touches `/proposals/` as the
  author's proposal, not by directly editing the doc).

### Branch protection configuration

Applied via `gh api` after files land:

```
main
├── require pull request: 1 approval baseline
├── dismiss stale approvals on new commits
├── require conversation resolution
├── require status check: lazy-majority-check (for paths: proposals/**)
├── block force pushes
├── block deletions
└── include administrators: false
```

### CI workflows

**`.github/workflows/lazy-majority.yml`**

- Trigger: `pull_request`, `pull_request_review` events.
- Only acts if PR changes files under `proposals/`.
- Reads PR approvals and blocking reviews via GitHub API.
- Reads PR `created_at`; computes age in days.
- Sets a commit status `lazy-majority-check` to `success` iff all three
  conditions met; otherwise `failure` with a descriptive message
  (e.g. "needs 2 more approvals; 3 days remaining in 7-day window").

**`.github/workflows/kb-index.yml`**

- Trigger: `push` to `main` touching `meetings/**`.
- Walks `meetings/*/README.md`, parses frontmatter (`date`, `title`, `tags`).
- Groups entries by tag; writes a rolled-up `knowledge-base/index.md`
  with per-tag sections listing linked entries.
- Commits the regenerated file back to `main` (uses `github-actions[bot]`).

## Content seeds

1. `proposals/0000-example.md` — a fictional but plausible "Shared Agent
   Eval Harness" proposal. `status: example` in frontmatter to mark it
   as a pattern-matching aid, not a real accepted proposal.
2. `meetings/2026-04-18-kickoff/README.md` — sparse but complete entry
   for the kickoff event. Serves as first real KB entry and template
   preview.
3. README points at both so a cold clone has visible material in every
   top-level directory.

## README restructure

New order:

1. Title + one-line description
2. "Hand this repo to your agent" callout with clone command
3. What this is (format: 50% hack / 50% unconference; 47th-in-agentic
   framing)
4. Who it's for
5. How this repo works (three-pillar overview with links to
   CONTRIBUTING/GOVERNANCE)
6. Volunteers (two sentences: Kat and Jordan jump-started; runs on
   whoever shows up; step up by opening an Issue or Discussion)
7. Tech stack (existing, preserved verbatim)
8. References (existing, preserved verbatim)

Event-specific content (dates, Luma links, fees) stays out of the repo.

## Operational migration checklist (manual, one-time)

Completed:
- [x] Create GitHub org `agentic-nwa`
- [x] Transfer `jordantcarlisle/agents-everywhere` → `agentic-nwa/agents-everywhere`
- [x] Update local git remote

Pending (handled in implementation plan):
- [ ] Enable Discussions on the repo
- [ ] Create "Ideas" Discussion category
- [ ] Apply branch protection ruleset via `gh api`
- [ ] Add Kat as a maintainer in `CODEOWNERS` and as org owner

## Success criteria

The design is successful if:

1. A new NWA community member can clone the repo, open it in their agent
   of choice, and get onboarded to the community and to their first
   contribution with no prior doc-reading.
2. Any member can submit a proposal in under 15 minutes with agent help.
3. Meeting notes land within the week following each meeting, tagged
   well enough that the rolled-up index is useful.
4. The first community-chosen project gets from "idea" to "accepted
   proposal" within 4 weeks of the repo going live.
5. At least one new volunteer maintainer is added within 2 months,
   without being asked.

## Open questions for implementation plan

- CI cost envelope: GitHub Actions is free for public repos; no budget
  issue expected. Flag if `lazy-majority-check` runtime exceeds ~10s.
- `CODEOWNERS` auto-requesting too many reviewers at once could be
  noisy. Start with just Kat + Jordan; let it grow organically.
- The agent-readable configs (`.cursor/rules/`, `copilot-instructions.md`)
  should be kept in sync with `AGENTS.md`. Consider a single source of
  truth file that the others include, or a CI check that fails if they
  drift. Defer to implementation plan.
