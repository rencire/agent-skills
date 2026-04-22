---
name: software-architecture
description: Use when designing, reviewing, or refactoring code architecture, especially large modules, unclear boundaries, dependency direction, or file splits. Turn architecture principles into small, behavior-preserving changes.
---

# Software Architecture

Use this skill for architecture reviews, refactors, and design decisions around
module boundaries.

## Default Lens

- Start from changeability: what is likely to change independently?
- Prefer deep modules: narrow public API, rich internals.
- Optimize for local reasoning and low coupling over generic reuse.
- Split along stable seams, not arbitrary line counts.

## Refactor Workflow

1. Identify the main source of complexity.
2. Name the responsibilities and ownership boundaries.
3. Pick the smallest coherent seam.
4. Move behavior, not just helpers; keep dependencies one-way.
5. Preserve behavior with tests or narrow checks.
6. Stop if the split adds indirection without reducing complexity.

## Heuristics

- Large files are a signal, not a verdict.
- If edits keep touching distant parts of one file, the module is too broad.
- If a helper needs lots of caller context, move the knowledge to the owner.
- Prefer one-way dependencies from stable policy to volatile details.
- Avoid `utils` and `misc` buckets unless they are genuinely generic.

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

See [references/principles.md](references/principles.md) for the concise
architecture checklist and the book-inspired heuristics behind this skill.
