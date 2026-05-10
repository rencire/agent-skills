---
name: rust
description: Use when designing module structure, crate boundaries, or idiomatic APIs in a Rust codebase. Covers ownership-aware interface design, visibility as an API tool, and Rust-specific split heuristics.
---

# Rust Architecture

Use this skill for Rust-specific structural decisions: crate and module layout,
ownership-aware API design, and idiomatic organization. For language-agnostic
module design principles, use the `software-architecture` skill. For system-level
decisions, use `system-architecture`.

## Default Lens

- `main.rs` is wiring only: parse args, build the dependency graph, hand off to library code.
- Separate the library crate (`lib.rs`) from the binary as early as useful — it enables testing without spawning a process.
- Module boundaries should align with ownership boundaries: who creates it, who drops it, who mutates it.
- Use visibility as a design tool, not just a compilation gate.

## Crate vs Module Boundaries

Split into separate **crates** when:
- the code has an independently useful public API
- build times justify parallel compilation
- a boundary must be enforced at the type system level (orphan rules)
- the piece will be published separately or versioned independently

Keep as **modules** within a crate when:
- the split would be purely organizational
- the pieces share internal types that would require re-exporting
- the boundary is not stable enough to warrant a versioned API

## Module Layout Heuristics

- `lib.rs` / `main.rs`: public API surface and wiring only — no business logic
- Group by domain responsibility, not by type kind (avoid `models/`, `handlers/`, `utils/` as top-level buckets)
- `pub(crate)` is the right default for internal sharing — reach for `pub` only when the item is part of the public API
- `pub(super)` signals an implementation detail shared between sibling modules
- Re-export from `lib.rs` to define the public API explicitly; do not let module paths leak into callers

## Ownership-Aware API Design

- Design for the common ownership pattern first: who creates the value, who uses it, who drops it
- Prefer borrowing (`&T`, `&mut T`) over cloning at module boundaries — clones at boundaries signal a missing abstraction
- If a function signature requires `Clone`, ask whether the caller should own the data or the module should
- Use `Arc<T>` at concurrency boundaries, not as a default for sharing
- Builder patterns are appropriate when constructing a value requires many optional fields; avoid when a struct literal is clear enough

## Trait-Based Abstraction

- Introduce a trait when you have two or more concrete implementations that callers should not distinguish between
- Avoid single-implementation traits — they add indirection without abstraction
- `impl Trait` in return position hides implementation details; use it when the concrete type is an implementation choice
- `dyn Trait` is appropriate when the concrete type is determined at runtime and the overhead is acceptable
- Keep trait methods minimal: a trait with one or two methods is easier to implement and compose than a wide trait

## Error Handling

- Define error types at the module boundary, not inside individual functions
- Use `thiserror` for library errors (stable, serializable error types)
- Use `anyhow` for application-level error propagation where the caller only needs to display or log the error
- Avoid `unwrap` and `expect` outside of tests and `main` — handle or propagate
- Design APIs so that fewer error states are possible (encode invariants in the type system)

## Visibility and Encapsulation

- The `pub` surface of a module is its contract — keep it as small as the callers need
- Internal types shared across sibling modules use `pub(crate)` or `pub(super)`
- Prefer newtype wrappers over type aliases when the distinction matters at the call site
- Seal traits that are not intended for external implementation using a private supertrait

## Practical Split Signals

- `main.rs` contains business logic
- A module has more `pub` items than its callers actually use
- Two modules share a private type by cloning it across the boundary
- A module's `use` imports span three or more unrelated domains
- A test requires constructing a complex internal type that is not part of the public API
