---
name: software-architecture
description: Use when designing, reviewing, or refactoring code architecture, especially large modules, unclear boundaries, dependency direction, or file splits. Turn architecture principles into small, behavior-preserving changes.
---

# Software Architecture

Use this skill for architecture reviews, refactors, and design decisions around
module boundaries. Scope: code and module level. For system-level decisions
(choosing architectural styles, service boundaries, deployment topology), use
the `system-architecture` skill instead.

## Default Lens

- Start from complexity: what is making this hard to change or understand?
- Prefer deep modules: narrow public API, rich internals.
- Pull complexity downward — make callers simple by absorbing it in the implementation.
- Optimize for local reasoning and low coupling over generic reuse.
- Split along stable seams, not arbitrary line counts.
- Design it twice: sketch two approaches before committing to one.

## Refactor Workflow

1. Identify the main source of complexity (change amplification, cognitive load, or unknown unknowns).
2. Name the responsibility and the design decision the module should own.
3. Pick the smallest coherent seam.
4. Move behavior, not just helpers; keep dependencies one-way.
5. Preserve behavior with tests or narrow checks before moving code.
6. Stop if the split adds indirection without reducing complexity.

## Heuristics

- Large files are a signal, not a verdict.
- If edits keep touching distant parts of one file, the module is too broad.
- If a helper needs lots of caller context, move the knowledge to the owner.
- Prefer one-way dependencies from stable policy to volatile details.
- Avoid `utils` and `misc` buckets unless they are genuinely generic.
- Pass-through methods that only delegate are a sign the module is too shallow.
- If two adjacent layers look the same, one of them is not pulling its weight.

## Good Extraction Targets

- page or UI composition vs shared widgets
- orchestration or state vs rendering
- persistence or parsing vs presentation
- transport or API adapters vs domain rules
- repeated transforms with a named responsibility

## When To Leave It Alone

- the module is large but conceptually single-purpose
- extracting it would create circular dependencies or a leaky API
- the current task is too small to justify the split

## Reference

See [references/principles.md](references/principles.md) for the full
architecture checklist and heuristics.

See [references/posd-chapters.md](references/posd-chapters.md) for chapter
summaries from *A Philosophy of Software Design* (Ousterhout), which is the
primary source for the principles in this skill.
