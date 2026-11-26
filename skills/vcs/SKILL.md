---
name: vcs
description: Use for version-control work in Git or Jujutsu repos, including commits, pushes, creating or updating pull requests, and formatting PR descriptions.
---

# VCS

Use this skill for everyday version-control tasks in Git or Jujutsu.

## Choose The Tool

- Use `git` when the repo is managed directly with Git.
- Use `jj` when the repo is managed with Jujutsu.
- If the repo uses Jujutsu on top of Git, do local history work with `jj` and publish with `jj git push` or `git push` when the Git view is already exported and current.

## Common Git Workflows

- Inspect: `git status`, `git diff`, `git log --oneline --graph --decorate --all`
- Start work: `git switch -c <branch>`
- Commit: `git add <paths>`, `git commit -m "<message>"`
- Amend: `git commit --amend`
- Move or rename a branch: `git branch -m <old> <new>`
- Merge: `git merge <branch>`
- Rebase: `git rebase <upstream>`
- Publish: `git push`, `git push -u origin <branch>`

## Common Jujutsu Workflows

- Inspect: `jj st`, `jj diff`, `jj log`
- Create a change: edit files, then `jj commit -m "<message>"`
- Update the bookmark to the commit you just made: `jj bookmark set <bookmark> -r @-`
- Equivalent bookmark move: `jj bookmark move <bookmark> -t @-`
- List bookmarks: `jj bookmark list`
- Repoint a bookmark manually: `jj bookmark set <bookmark> -r <revision>`
- Rebase or reshape history: `jj rebase`, `jj new`, `jj squash`, `jj split`
- Export to the underlying Git repo: `jj git export`
- Publish: `jj git push --bookmark <bookmark>` or `jj git push --all`
- Sync from Git: `jj git fetch`, `jj git import`

## Practical JJ Pattern

1. Edit files.
2. Run `jj commit -m "<message>"`.
3. Move the bookmark to the commit you just made with `jj bookmark set <bookmark> -r @-`.
4. Push with `jj git push --bookmark <bookmark>`.
5. If you need to use raw `git push`, run `jj git export` first.

## Pull Requests

1. Inspect repository instructions, remotes, the target branch, and the complete proposed diff. Look for PR templates before writing a description.
2. Write the body as real Markdown: headings, short paragraphs, and bullet points. Follow the repository template; otherwise use Summary and Verification, plus Deployment or Follow-up when relevant. Separate completed checks from pending checks and deployment. Do not claim activation from a build or manual smoke test.
3. Put multiline bodies in a file or a tool's structured body field. For APIs, use a JSON serializer rather than hand-escaped JSON. Literal `\n` text is not a Markdown newline.
4. Publish the intended commits and actually create or update the PR using the forge's supported interface. A branch push or compare URL alone is not a PR. For Forgejo, read [the Forgejo reference](references/forgejo-pull-requests.md).
5. Read the saved PR back from the server. Verify its URL, target branch, head commit, and complete body, including line breaks. On an update, confirm the existing PR changed rather than creating a duplicate. If authentication or verification is unavailable, report that blocker rather than claiming completion.

Push, merge, and deployment are separate actions: obtain the applicable authorization for each. Keep explicit deployment holds visible in the PR.

## Guardrails

- Do not assume Git branches and Jujutsu bookmarks are interchangeable.
- If you rewrite history in `jj`, verify the bookmark still points at the intended commit before pushing.
- Prefer one tool as the source of truth for the current operation; do not mix `git` staging and `jj` history edits unless the repo workflow already does that.
