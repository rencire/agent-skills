---
name: design-doc
description: Use when writing a design doc or RFC for a non-trivial change. Picks the doc tier, structures it (Context, Goals/Non-Goals, Design, Alternatives Considered, Cross-Cutting Concerns), and handles decisions that don't fit in one section.
---

# Design Doc

Use this skill before writing a design doc or RFC, and when reviewing one for
structure. Scope: how to shape the document. For the underlying architecture
judgment, use the `software-architecture` or `system-architecture` skills
instead.

## Pick The Tier

- **Skip the doc.** The change is unambiguous, small, or has one obviously
  correct approach.
- **Mini design doc (~1 page).** One real decision, low blast radius, no
  cross-team impact. Write it inline in the repo's normal doc location.
- **Full design doc.** Multiple interacting concerns, meaningful trade-offs,
  or other people need to weigh in before implementation starts.
- **RFC + separate design doc.** The decision itself needs a durable,
  independently-citable record because it may be revisited or rolled back
  later. Split *why we decided this* (RFC) from *how it works now*
  (design doc).

Match formality to ambiguity, not to a template's section count.

## Standard Sections

- **Context and Scope**: background facts needed to understand the problem.
- **Goals and Non-Goals**: what the design will and deliberately will not do.
  This is the section most often skipped and the one that most keeps scope
  and alternatives-debate anchored.
- **The Design**: overview first, details after; lead with trade-offs, not
  a spec.
- **Alternatives Considered**: options that were rejected, and why, not just
  a list of names.
- **Cross-Cutting Concerns**: security, rollback/blast radius, operational
  impact.

Skeleton:

```md
# <Title>

## Context and Scope

## Goals and Non-Goals

## The Design

## Alternatives Considered

## Cross-Cutting Concerns
```

## Optional Techniques

Reach for these only when they earn their keep. They aren't tiers to pick one
from. Mix them based on how entangled the decisions are. None of this is
prescribed by the sources below; it's patterns that have worked in practice.

- **Enumerate sub-decisions inside one section.** When a design bundles
  several related choices (for example, breaking a `Decision` section into
  short bullets for service ownership, data boundary, socket access, and
  rollback path, instead of prose that blurs them together), list each one so
  it gets reasoned through individually without the overhead of separate
  documents.
  - Give each sub-decision a short label. Use one or two words naming it,
    not a full clause.
  - Follow the label with one plain-language sentence. State what it does
    and why, in terms a non-expert could follow.
  - Put technical detail after that. Option names, paths, flags, and
    commands come last.
  - Use this whether the sub-decision is a `###` heading with a lead
    paragraph, or a bold label opening a bullet. The mechanism doesn't
    matter.
  - A reader skimming just the labels and lead sentences should get the
    shape of the section.
  - Reach for this only when a section is enumerating sub-decisions.
    Sections like `Context and Scope` or `Goals and Non-Goals` are fine as
    plain prose or a flat list.
- **Split into standalone RFC-style decision docs.** When decisions are
  independently significant and likely to be revisited or cited on their own
  later, give each one its own short RFC (Problem, Decision, Alternatives,
  Consequences) rather than folding it into one design doc's subsection.
- **One global Alternatives Considered.** For designs with a single real
  decision at stake, don't add structure beyond the standard sections above.

## Writing Style

- Prose, not a spec-manual. State trade-offs and reasoning; that's the part
  a template can't generate for you.
- Skip code or pseudocode unless it's essential to conveying the design.
- Preserve open questions explicitly rather than pretending an unresolved
  detail is decided.
- Define industry terms (control plane, blast radius, source of truth, ...)
  through the concrete behavior being described, in the same paragraph. Don't
  add a textbook explanation next to them.

## RFC + Design Doc Pair

When the tier calls for a split:

- **RFC**: Summary, Problem, Decision, Alternatives, Consequences, Resolved
  Questions. Frozen once decided: a record of the decision and why, not
  touched again even as the implementation evolves.
- **Design doc**: `Status` (Draft/Implemented/Superseded) and a `Related RFC`
  link at the top, then implementation sections (architecture, invariants,
  rollback). Stays valid as long as it describes the current architecture:
  correct it in place for details that don't change the decision. If a new
  decision changes the architecture, don't rewrite it to describe two
  generations of the system: write a new RFC and design doc, and mark this
  one `Superseded` with a link forward.

## Reference

See [references/sources.md](references/sources.md) for the industry material
this skill is based on.
