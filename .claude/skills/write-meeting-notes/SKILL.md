---
name: write-meeting-notes
description: Use when a community member wants to write up what happened at an Agents Everywhere meeting. Creates a dated meeting folder and walks them through the template.
---

# write-meeting-notes

When the user wants to write up a meeting:

0. **If they haven't introduced themselves yet, and you haven't already
   offered in this conversation, offer now.** Check whether
   `community/members/<github-handle>.md` exists. If not — and if
   AGENTS.md's intro step didn't already run earlier in the session —
   offer a quick profile PR as a 2-minute first commit using
   `community/members/TEMPLATE.md` (3 short questions). Otherwise, skip
   and proceed to the flow below.
1. Read `meetings/TEMPLATE.md`.
2. Ask for the meeting date (YYYY-MM-DD) and a short title.
3. Pick a slug (kebab-case) and create the folder
   `meetings/YYYY-MM-DD-<slug>/`. Copy `meetings/TEMPLATE.md` to
   `meetings/YYYY-MM-DD-<slug>/README.md`.
4. Walk them through the 5 questions one at a time:
   - "In a few lines, what was the overall vibe and shape of the day?"
   - "Who led something, and what was it about?"
   - "Anyone demo a project? What and what stood out?"
   - "Any tools, papers, repos, or threads that got shared?"
   - "Anything the group wants to pick up next time?"
5. Fill in their answers as you go.
6. At the end, suggest tags based on what got discussed. Confirm with
   them, then write the tag list into the frontmatter `tags: [...]`.
7. If they mention demos or slides, create `./demos/` and `./slides/`
   subfolders and remind them to drop materials in.
8. Open a PR. Meeting notes don't need lazy-majority acceptance — a
   single maintainer review is enough to merge.
