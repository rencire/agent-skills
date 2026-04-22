# Architecture Principles

This skill is inspired by common themes in architecture writing such as
*A Philosophy of Software Design*, *Clean Architecture*, and related design
guides. The goal is not to repeat any one book, but to convert the shared
themes into practical review heuristics.

## Core Themes

- minimize complexity before maximizing abstraction
- keep interfaces smaller than the implementation beneath them
- localize knowledge so change stays inside one module when possible
- favor cohesion inside a module and low coupling between modules
- make dependencies point from policy toward detail, not the other way around
- choose names that reveal responsibility, not implementation accidents

## Architecture Review Checklist

1. What is the unit of change?
2. What knowledge does this module own?
3. Is the public surface smaller than it needs to be?
4. Can the dependency direction be made one-way?
5. Would a split reduce cognitive load, or just add files?
6. Can behavior be pinned with tests before moving code?

## Practical Split Signals

- one file mixes composition, state, and persistence
- the same helper is pulled in from many unrelated callers
- a module has multiple reasons to change for unrelated features
- the public API is broad but the internal behavior is narrow
- reading the file requires bouncing between distant regions

## Practical Split Targets In Rust Apps

- keep `main.rs` as app shell and wiring
- move data access and persistence into focused modules
- move page-level UI into dedicated view modules
- keep shared models and transforms close to the data they shape
- expose small, intentional module APIs instead of wide catch-alls
