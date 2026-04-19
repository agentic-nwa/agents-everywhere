---
name: review-proposals
description: Use when a community member wants to review and vote on open RFPs. Lists open proposals, summarizes each, and walks them through voting.
---

# review-proposals

When the user wants to review open proposals:

0. **Intro only if not done yet.** If
   `community/members/<github-handle>.md` doesn't exist and AGENTS.md's
   intro step didn't already run earlier in the session, offer a 2-minute
   profile PR using `community/members/TEMPLATE.md`. Otherwise skip.
1. Run: `gh pr list --repo agentic-nwa/agents-everywhere --state open --search "files:proposals/"`.
2. For each open PR, read the proposal file on the PR branch:
   `gh pr view <number> --json files,body,title,createdAt,reviews`.
3. Give the user a one-paragraph summary of each proposal: what it is,
   why the author says it matters, who's committing to build it.
4. For each, show:
   - Current approval count
   - Any blocking reviews (with reasons)
   - Days since the PR was opened (and days remaining in the 7-day window)
5. Ask the user which they want to weigh in on.
6. For their choice, help them either:
   - Approve: `gh pr review <number> --approve -b "<reason>"`
   - Block: `gh pr review <number> --request-changes -b "<reason>"`
     (require a reason — blocks without reasons are ignored)
   - Comment: `gh pr review <number> --comment -b "<text>"`
7. Point them at [`GOVERNANCE.md`](../../../GOVERNANCE.md) for the
   lazy-majority rule. The `lazy-majority-check` CI check enforces it
   mechanically.
