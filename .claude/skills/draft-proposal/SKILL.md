---
name: draft-proposal
description: Use when a community member wants to propose something for Agents Everywhere to build. Walks them through the RFP template one question at a time and opens a PR.
---

# draft-proposal

When the user wants to propose something to the community:

0. **If they haven't introduced themselves yet, and you haven't already
   offered in this conversation, offer now.** Check whether
   `community/members/<github-handle>.md` exists. If not — and if
   AGENTS.md's intro step didn't already run earlier in the session —
   offer a quick profile PR as a 2-minute first commit using
   `community/members/TEMPLATE.md` (3 short questions). Otherwise, skip
   and proceed to the flow below.
1. Read `proposals/TEMPLATE.md` and `proposals/0000-example.md`.
2. Ask the 6 questions in the template, ONE at a time. Don't batch.
   - "In one or two sentences, what is this?"
   - "Who benefits, and why now?"
   - "What would this actually look like — architecture, UX, a paragraph?"
   - "Who's committing to work on this? Names, rough hours. 'Just me' is fine."
   - "What don't you know yet? Where do you need help?"
   - "Based on the shape you described, this feels like [in-repo|new-repo].
     Agree, or does the other fit better?" (in-repo for small
     tools/notebooks/shared libs; new-repo for standalone products.)
3. Help them pick a short slug for the filename (kebab-case, 2–4 words).
4. Write the draft to `proposals/YYYY-MM-DD-<slug>.md` using today's date
   and their answers. Fill in frontmatter (`title`, `author`, `scope`,
   `status: draft`).
5. Read it back to them. Ask if they want to tighten anything.
6. Offer two paths:
   a) Post to [Discussions](../../discussions) first to gauge interest, then PR.
   b) Go straight to PR if they already have committed collaborators.
7. If (b), create a branch, commit, push, and open the PR with
   `gh pr create`. The proposal PR template will apply automatically.
8. Point them at [`GOVERNANCE.md`](../../../GOVERNANCE.md) for the
   lazy-majority rule. The `lazy-majority-check` CI check enforces it
   mechanically.
