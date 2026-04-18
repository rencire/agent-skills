---
name: dev-loop
description: Use for non-trivial implementation work before committing: clarify behavior, test first when appropriate, implement narrowly, verify, refactor, re-verify, scan for secrets/security issues, update docs, then commit.
---

# Dev Loop

Use this skill for non-trivial implementation work.

## Workflow

1. Clarify intended behavior when needed.
2. If this is library or application code and the behavior is testable, write or
   update a failing test first.
3. Implement the smallest clear change that satisfies the behavior.
4. Add comments only where context is not obvious from the code.
5. Run the narrowest meaningful tests or checks.
6. Once green, refactor for clarity and concision.
7. Rerun the same tests or checks after refactoring.
8. Run publication and security hygiene:
   - inspect the tracked diff
   - scan for secrets, private keys, tokens, local paths, and generated
     artifacts
   - use `public-repo-readiness` before pushing when relevant
9. Update docs, prompt logs, or runbooks when behavior or workflow changed.
10. Commit one logical change with a conventional commit message before
    finishing, unless the user explicitly asked not to commit or verification
    failed in a way that should remain visible.

## Guardrails

- Do not force TDD for config-only or deployment-only changes where tests are
  not meaningful.
- Prefer narrow verification before broad expensive checks.
- Do not mix unrelated cleanup with the behavior change.
- Do not leave complete, verified work uncommitted at the end of a turn. If the
  worktree already has unrelated edits, split or commit only your logical
  change.
- Do not commit if verification failed unless the gap is documented and
  accepted.
