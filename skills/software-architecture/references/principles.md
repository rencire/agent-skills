# Architecture Principles

Distilled from *A Philosophy of Software Design* (Ousterhout) and related
design writing. See [posd-chapters.md](posd-chapters.md) for the chapter
summaries behind these heuristics.

## Core Themes

- Complexity is the root problem; every design decision either reduces it or adds to it
- Strategic programming: treat every change as an investment in the design, not just a task to complete
- Deep modules: small, simple interfaces hiding large, rich implementations
- Information hiding: each module owns a design decision; callers should not see inside
- Pull complexity downward: absorb it at the implementation so callers stay simple
- Different layer, different abstraction: adjacent layers that look the same signal a missing or redundant layer
- Design it twice: sketch two approaches before committing; the comparison reveals what matters

## Complexity Symptoms to Watch For

- **Change amplification** — a single logical change requires edits in many places
- **High cognitive load** — understanding a module requires holding too much context
- **Unknown unknowns** — it's unclear what needs to change or where

## Module Design Heuristics

- Prefer fewer, deeper modules over many small, shallow ones
- A shallow module is one whose interface is nearly as complex as its implementation — it adds layers without hiding complexity
- If the common case requires callers to know too much, the module is too shallow or leaking internals
- Ask: if the next caller had a slightly different need, would this interface still serve them? (generality test)
- Pass-through methods that only delegate are a signal the module is too thin
- Modules that are always used together often share hidden knowledge — consider merging

## Information Hiding

- Each module should encapsulate one design decision
- Temporal coupling (two modules that must be used in order) is hidden dependency — internalize the sequence
- When a design decision appears in multiple modules, a change to that decision ripples everywhere

## Splitting and Merging

- Split when: pieces have independent reasons to change, or one can be fully understood without the other
- Keep together when: pieces share information, are always used together, or overlap conceptually
- Never split just to keep files small — that is complexity redistribution, not reduction
- A split that adds indirection without reducing cognitive load is not an improvement

## Pulling Complexity Down

- If a module and its callers must share complexity, the module should absorb it
- Exposing configuration knobs "for flexibility" usually means exporting complexity to every caller
- Handle errors at the lowest level that has enough information to handle them correctly
- Design APIs so that fewer error states are possible, not just so errors are propagated cleanly

## Naming and Obviousness

- Names are abstractions — a good name conveys meaning at the right level, not too specific and not too vague
- If you cannot find a precise name for something, the thing is probably not well-defined yet
- Obvious code can be read without referring to anything else: good names, consistent conventions, no action at a distance
- Comments describe the *why*, the *intent*, and the *invariants* — not a summary of what the code already says

## Consistency

- Once a pattern is established, follow it everywhere — inconsistency forces readers to hold multiple mental models
- Only break consistency when there is a compelling and visible reason; document that reason

## Architecture Review Checklist

1. What is the unit of change, and does a single logical change stay inside one module?
2. What design decision does this module own, and is it fully hidden?
3. Is the public surface smaller than the implementation beneath it?
4. Would the next caller with a slightly different need find this interface usable?
5. Are adjacent layers providing genuinely different abstractions?
6. Can the complexity live lower so callers stay simpler?
7. Did we consider at least two designs before committing?
8. Would a reader be surprised by what this code does?

## Practical Split Signals

- One file mixes composition, state, and persistence
- The same helper is imported by many unrelated callers
- A module has multiple reasons to change for unrelated features
- Reading the module requires bouncing between distant regions
- Edits to one feature routinely touch code owned by another
