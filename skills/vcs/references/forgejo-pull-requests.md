# Forgejo Pull Requests

## Choose an authenticated interface

Check the repository's Forgejo version and supported interfaces. SSH push access does not imply API authentication. Prefer an existing authenticated forge tool, API client, or browser session; never print credentials or put them in tracked files. If no suitable interface is available, report what is blocked and ask for access.

## Create over SSH with AGit

Where AGit is enabled, Git can create a PR without API credentials:

```sh
git push <remote> HEAD:refs/for/<target>/<topic> \
  -o title='Concise change title' \
  -o description='Short initial summary.'
```

Replace the placeholders after checking the remote and target. Record the returned PR URL. The topic identifies the AGit PR; push later code updates to the same target/topic. An ordinary feature-branch push may not update that PR's head.

Git push options reject literal newlines. Do not pass a multiline file through `-o description="$(cat pr-body.md)"`, or assume backslash-n escapes become line breaks. Use an authenticated API/tool/UI to save the final Markdown body. If only SSH is available, explain that the readable-body step is still blocked.

Repeated AGit description push options do not reliably edit an existing PR body. Use the update interface below instead.

## Create or update through the API

Use the instance's API documentation for authentication and supported fields:

- Create: `POST /api/v1/repos/{owner}/{repo}/pulls` with `head`, `base`, `title`, and `body`.
- Update the description: `PATCH /api/v1/repos/{owner}/{repo}/pulls/{index}` with `body` (and `title` only if intended).
- Read back: `GET /api/v1/repos/{owner}/{repo}/pulls/{index}`.

Prepare a real multiline `pr-body.md` outside the commit set, for example:

```markdown
## Summary

Refresh the cache using the shared refresh operation.

## Verification

- Unit tests passed.
- Packaged CLI smoke test passed on the target host.

## Deployment

- Scheduled service activation is pending approval.
```

Adapt every statement to actual evidence. Serialize the file into a JSON request, for example:

```sh
jq -Rs '{body: .}' < pr-body.md > pr-update.json
```

Submit that payload with the authenticated client. Do not invent authentication, provision privileged tokens as a fallback, or infer API write access from a successful SSH push.

Read the saved PR back and compare the returned `body` exactly with the intended file contents. Check the target branch and head SHA as well; a successful HTTP response alone does not establish that the intended PR and commits were updated. Return the actual PR URL and clearly identify any unfinished step.
