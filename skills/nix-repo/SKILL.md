---
name: nix-repo
description: "Use for implementation work in Nix-managed repos: read local repo docs first and keep Nix-specific environment guidance separate from the generic dev loop."
---

# Nix Repo

Use this skill for repo-local implementation work in a Nix-based project.

## Workflow

1. Read the repo's local guidance first, especially `AGENTS.md` and any
   workflow doc it points to.
2. Assume the harness starts inside the repo dev shell. If not, enter it once
   with `nix develop`, then run project commands normally.
3. If a required tool is missing from the shell, propose adding it to
   `flake.nix` and get approval before expanding the dev environment.
4. Use the generic `dev-loop` skill for the implementation workflow itself.

## Homebrew vs. Nix packages

- `nixpkgs.config.allowUnfree = true` only applies to Nix-managed packages. Homebrew casks (declared under `homebrew.casks`) install independently and do not require this setting — unfree casks like `google-chrome` work without it.

## Guardrails

- Do not expand the dev shell without approval.
