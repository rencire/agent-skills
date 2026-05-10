# A Philosophy of Software Design — Chapter Summaries

Distilled from John Ousterhout's *A Philosophy of Software Design*.
These summaries capture the intent of each chapter as practical heuristics,
not as a reproduction of the original text.

---

## Ch 1 — Introduction: It's All About Complexity

The central challenge of software development is managing complexity. Complexity
grows as systems evolve. The only way to remain productive long-term is to treat
every change as an opportunity to reduce it, not just add features. Two approaches
exist: make code simpler and more obvious, or encapsulate it so others don't have
to deal with it.

## Ch 2 — The Nature of Complexity

Complexity has three symptoms:
- **Change amplification** — a simple change requires edits in many places.
- **Cognitive load** — the developer must hold too much in their head to make a change safely.
- **Unknown unknowns** — it's not clear what needs to change or where.

Complexity has two root causes: **dependencies** (one piece of code relates to
another in ways that must be managed together) and **obscurity** (important
information is not obvious). The goal of design is to reduce both.

## Ch 3 — Working Code Isn't Enough: Strategic vs Tactical Programming

**Tactical programming** ships features fast by taking the easiest path. It
creates technical debt incrementally and eventually makes future changes slow
and dangerous.

**Strategic programming** treats every change as an investment in the design.
It takes a little more time now to make future changes easier. The best
engineers default to strategic thinking even under time pressure. A 10–15%
time investment in design typically pays for itself quickly.

## Ch 4 — Modules Should Be Deep

A module is any unit with an interface and an implementation (function, class,
subsystem). **Deep modules** have a small, simple interface and a large,
rich implementation — they hide a lot of complexity behind a narrow API.
**Shallow modules** have interfaces nearly as complex as their implementation —
they provide little abstraction benefit and increase cognitive load.

Prefer fewer, deeper modules over many small ones. The best interface is one
that makes the common case require as little knowledge as possible.

## Ch 5 — Information Hiding (and Leakage)

Every module should encapsulate a design decision. Callers should not need to
know how the module works, only what it promises. **Information leakage** occurs
when a design decision is reflected in multiple modules — changes then ripple
everywhere. Watch for temporal coupling: two modules that must always be used in
a specific sequence share hidden knowledge that should be internalized.

## Ch 6 — General-Purpose Modules Are Deeper

Modules designed for a specific use case often end up with leaky, narrow
interfaces. Modules designed slightly more generally tend to have cleaner
interfaces and are reused more. The sweet spot is *somewhat general-purpose*:
not so general that the interface is vague, but general enough that the interface
survives the first caller. Ask: if the next caller had a slightly different need,
would this interface still work?

## Ch 7 — Different Layer, Different Abstraction

Each layer of a system should provide a clearly different abstraction from the
layers above and below it. If two adjacent layers look similar — exposing the
same concepts at the same granularity — one of them is not pulling its weight.
**Pass-through methods** (methods that just delegate to a lower layer with no
added logic) are a signal that a module is too shallow and should be merged
or redesigned.

## Ch 8 — Pull Complexity Downward

When a module and its callers must share some complexity, it is better for the
module to absorb it. Callers are more numerous than implementations. Making a
module slightly more complex so that every caller is simpler is almost always
the right trade. Resist the temptation to expose configuration knobs or push
decisions up to the caller "for flexibility" — that is just exporting complexity.

## Ch 9 — Better Together or Better Apart?

The key question when splitting or merging modules: does the split reduce overall
complexity, or just redistribute it? Signs that things should stay together:
they share information, are always used together, or overlap conceptually.
Signs they should be split: they have independent reasons to change, or one can
be understood without the other. Splitting for its own sake (to keep files small)
often adds indirection without reducing complexity.

## Ch 10 — Define Errors Out of Existence

Exception handling is one of the worst sources of complexity. The best approach
is to design APIs so that fewer errors are possible — reduce the number of places
where an error can be thrown. Where errors must exist, handle them at the lowest
level that has the information to handle them correctly. Avoid surfacing errors
to callers unless the caller can actually do something useful with them.

## Ch 11 — Design It Twice

Before committing to a design, sketch at least two genuinely different
approaches. The comparison reveals which properties matter and which are
incidental. Even experienced engineers produce better designs when they force
themselves to consider an alternative. "The first design is usually not the best."

## Ch 12 — Why Write Comments? The Four Excuses

Good code is not self-documenting. The four common excuses for not writing
comments ("good code explains itself", "no time", "comments go stale",
"the tests document it") all collapse under scrutiny. Comments capture the
**why** and the **what was considered** — things that cannot be read from code
alone.

## Ch 13 — Comments Should Describe Things Not Obvious from the Code

Comments are not a summary of the code. They should describe the **intent**,
the **invariants**, the **constraints**, and the **why**. If a comment just
restates what the code already says, delete it. The highest-value comments
describe interface contracts, non-obvious preconditions, and design decisions
that would surprise a future reader.

## Ch 14 — Choosing Names

Names are a form of abstraction. A good name conveys meaning precisely and
at the right level of abstraction. Names that are too specific tie the code to
a single use; names that are too vague (e.g. `manager`, `handler`, `processor`)
convey nothing. If you can't find a precise name for something, the thing
probably isn't well-defined yet.

## Ch 15 — Write the Comments First

Writing the interface comment before the implementation forces you to think
about what the module promises before you think about how it works. If the
comment is hard to write, the interface is probably wrong. Comments-first is
a design tool, not just a documentation habit.

## Ch 16 — Modifying Existing Code

When changing code, resist the "tactical" impulse to make the smallest possible
edit. Ask whether the change fits the existing design or whether the design
should be updated. If adding a feature requires touching many places, the
system is signaling a missing abstraction. Improving the design as you go is
the only sustainable way to keep a codebase healthy.

## Ch 17 — Consistency

Consistent conventions reduce cognitive load. Once a pattern is established
(naming, error handling, module structure), follow it everywhere — even if you
disagree with the original choice. Inconsistency forces readers to hold multiple
mental models simultaneously. Only break consistency when there is a compelling,
visible reason to do so.

## Ch 18 — Code Should Be Obvious

Obvious code can be read and understood without the reader needing to refer to
anything else. Two main techniques: choose good names and follow conventions.
Things that make code non-obvious: event-driven flow, global state, action at a
distance, generic containers used without type aliases. If a reader would be
surprised by what a piece of code does, it needs improvement.

## Ch 19 — Software Trends

Unit tests, test-driven development, design patterns, getters/setters, and
agile processes are evaluated against the complexity criterion. Useful when they
reduce complexity; harmful when applied dogmatically. TDD can encourage tactical
thinking if tests drive implementation rather than design. Design patterns are
good vocabulary but not destinations. The question is always: does this reduce
complexity?

## Ch 20 — Designing for Performance

Performance and clean design are not inherently in conflict. First, measure.
Then optimize at the module boundary where the cost is concentrated. Spreading
performance hacks throughout the codebase is the complexity equivalent of
premature optimization — it costs readability everywhere to gain speed nowhere
in particular.

## Ch 21 — Conclusion

The most important skill is developing a sense for what complexity looks like
and the discipline to address it incrementally. Every design decision either
deposits into or withdraws from the complexity budget. Strategic programmers
treat the codebase as something to steward, not just ship.
