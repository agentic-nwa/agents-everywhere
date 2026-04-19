# Instructions for agents working in this repo

You are helping a member of the Agents Everywhere community — an NWA-based
group of builders actively shipping AI agents. This repo is three things
at once: a knowledge base, an RFP system, and the home of whatever
open-source project the community decides to build together.

## Your job right now

The human just cloned this repo or pointed you at it. Before anything
else, ask:

> **Have you introduced yourself yet?**

Check whether `community/members/<their-github-handle>.md` exists. If it
doesn't, offer to create one — it's a 2-minute first commit and the
lowest-barrier way to join the repo.

If they take you up on it:

1. Ask three short questions, one at a time (prompts live in
   `community/members/TEMPLATE.md`):
   - Who are you? (one sentence)
   - What are you building right now?
   - What are you into — what do you want to learn or collaborate on?
2. Write their answers into `community/members/<handle>.md` using the
   template.
3. Open a PR. Done. A member profile PR counts — not everyone needs to
   draft a proposal to belong here.

If they already have a profile, or they decline, move on.

## Now that you're in, here's what most people do next

Ask which of these they want — don't guess:

1. **Draft a proposal** — propose something the community should build.
   Use the `draft-proposal` skill if you have it; otherwise walk them
   through `proposals/TEMPLATE.md` one question at a time (look at
   `proposals/0000-example.md` for a worked example).

2. **Browse the idea backlog** — open `proposals/IDEAS.md`. These are
   unclaimed ideas kicking around. If one grabs them, they can champion
   it: turn it into a formal RFP in `/proposals/`.

3. **Write meeting notes** — they attended a session. Use the
   `write-meeting-notes` skill if you have it; otherwise read
   `meetings/TEMPLATE.md`, create `meetings/YYYY-MM-DD-<slug>/README.md`,
   and help them fill it in with frontmatter tags.

4. **Review open proposals** — they want to vote. Use the
   `review-proposals` skill if you have it; otherwise list open PRs that
   touch `/proposals/`, summarize each, and show them how to 👍 or leave
   a blocking review with a reason.

5. **Understand the community** — they're new. Walk them through
   `README.md`, then `GOVERNANCE.md`, then come back here.

If their request doesn't match any of these, ask what they want to do.

## Ground rules

- Never push to `main`. All work goes through PRs.
- Member profiles go in `/community/members/<handle>.md`.
- Proposals go in `/proposals/`.
- Meeting notes go in `/meetings/YYYY-MM-DD-<slug>/`.
- Tag meeting notes so the KB index can roll them up.
- Follow `CONTRIBUTING.md` for commit style and PR etiquette.
- When filling in a template, read the `<!-- AGENT: Ask — ... -->`
  comments in each section. Those are your question prompts. Ask one at
  a time; fill in the answers as you go.

## Lazy-majority rule (for proposals only)

See [GOVERNANCE.md](GOVERNANCE.md) — the `lazy-majority-check` GitHub
Action enforces it mechanically.
