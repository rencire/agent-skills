---
name: nix-repo-dev-loop
description: Use for implementation work in Nix-managed repos: read the local repo docs first, run commands through nix develop, verify narrowly, update docs when behavior changes, scan for publication risks, and commit one logical change.
---

# Nix Repo Dev Loop

Use this skill for repo-local implementation work in a Nix-based project.

## Workflow

1. Read the repo's local guidance first, especially `AGENTS.md` and any
   workflow doc it points to.
2. Run project commands through `nix develop -c ...` so you work inside the
   repo's declared environment.
3. Prefer the narrowest failing test or check that proves the change.
4. Implement the smallest coherent change that satisfies the behavior.
5. Add brief, behavior-focused comments only when code would otherwise be hard
   to follow.
6. Re-run the same checks after any refactor.
7. Review the tracked diff for secrets, private keys, tokens, machine-local
   paths, and generated artifacts.
8. Update docs, runbooks, or prompt logs when behavior or workflow changes.
9. Use `public-repo-readiness` before pushing when publication risk matters.
10. Commit one logical change with a conventional commit message before
    finishing, unless the user asked not to or verification failed in a way
    that should remain visible.

## Guardrails

- Do not expand the dev shell without approval.
- Do not mix unrelated cleanup with the behavior change.
- Do not leave complete, verified work uncommitted at the end of a turn.
- If the repo has a separate commit policy, follow the repo policy and keep the
  change focused.
