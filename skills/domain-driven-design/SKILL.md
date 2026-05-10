---
name: domain-driven-design
description: Use when modeling a business domain, naming domain concepts, drawing aggregate or bounded context boundaries, or reviewing whether code structure reflects domain intent. Applies DDD building blocks and strategic design patterns.
---

# Domain-Driven Design

Use this skill when working on domain modeling: naming concepts, defining
aggregate boundaries, structuring bounded contexts, or reviewing whether
code reflects the domain accurately. For module-level design principles,
use `software-architecture`. For system-level style choices, use
`system-architecture`.

## Default Lens

- The model is in the code — if the code does not express the domain, the
  model does not exist.
- Use the Ubiquitous Language in every identifier, test name, and conversation.
  Divergence between code and domain language is a defect, not a style choice.
- Start with the Core Domain — the part of the system that is the business's
  competitive advantage — and invest design effort there first.
- Prefer value objects over entities; prefer small aggregates over large ones.
- Protect bounded context boundaries; translate at the edges.

## Modeling Workflow

1. Identify the Ubiquitous Language for this context — what terms do domain
   experts actually use?
2. Classify domain objects: entity (identity matters), value object (attributes
   define it), or domain service (stateless operation on the domain).
3. Find the consistency boundaries — what invariants must hold within a single
   transaction? That is your aggregate boundary.
4. Identify the aggregate root — the single entry point that enforces invariants.
5. Define repository interfaces (one per aggregate root) in the domain layer.
6. Map bounded contexts and choose integration patterns at each boundary.
7. Identify the Core Domain and ensure it is receiving proportionate investment.

## Building Block Heuristics

**Entities**
- Needs a stable identity across its lifetime
- Two instances with the same attributes are still distinct
- Minimize mutable state to what the domain requires

**Value Objects**
- No identity — defined entirely by attributes
- Must be immutable; replace, do not mutate
- Use for: money, measurements, dates, addresses, names, identifiers

**Aggregates**
- Draw the boundary at the consistency requirement, not the data relationship
- External objects reference the root only, never internal members
- Keep small — one to three entities is often right
- Reference other aggregates by identity (ID), not by object reference
- One transaction, one aggregate — if a use case needs two aggregates, reconsider
  the boundary or accept eventual consistency

**Domain Services**
- No state; operates on domain objects
- Named with a Ubiquitous Language verb
- Use sparingly — most logic belongs on entities or value objects

**Repositories**
- One per aggregate root, no more
- Returns fully reconstituted aggregates, not raw data
- Interface in the domain layer; implementation in infrastructure

## Layered Architecture Checklist

- Domain layer: zero dependencies on infrastructure, frameworks, or the
  application layer
- Application layer: orchestrates use cases; contains no business rules
- Infrastructure layer: implements repository interfaces; knows about databases
- If business logic appears in a controller, handler, or service outside the
  domain layer, move it

## Strategic Design Heuristics

**Bounded Contexts**
- One model, one language per context — enforce the boundary in code
- Name contexts after the business area they represent
- Ambiguity between contexts is fine; ambiguity within a context is a defect

**Context Integration Patterns**
- Upstream you do not control + model you want to protect → **Anticorruption Layer**
- Two teams sharing a small stable model subset → **Shared Kernel**
- Downstream with negotiating power → **Customer/Supplier**
- Upstream you cannot influence → **Conformist** (adopt their model)
- Many consumers of a stable API → **Open Host Service / Published Language**

**Core Domain**
- Identify which subdomain is the business's competitive differentiator
- Generic subdomains (auth, billing, email): buy or use off the shelf
- Supporting subdomains: build simply, do not over-invest
- Core Domain: invest here; this is what makes the software worth building

## Signals the Model Needs Work

- The code uses different names than domain experts do
- Entities have no meaningful identity beyond their database key
- Aggregates are large and routinely loaded in full to change one thing
- Business rules are scattered across application services, not on domain objects
- Cross-context calls bypass translation (no anticorruption layer)
- A "service" class contains all the business logic and entities have only getters
- Terms like `Manager`, `Handler`, or `Processor` appear without domain meaning
- Invariants are enforced in multiple places instead of inside one aggregate

## When to Reach for DDD

DDD pays off when the domain is genuinely complex and central to the product's
value. For simple CRUD applications, thin wrappers around external services,
or tooling with no domain rules, the overhead of full DDD is not worth it.
Apply selectively: the Ubiquitous Language and bounded contexts are almost
always useful; the full building block taxonomy is most valuable in the Core
Domain.

## Reference

See [references/principles.md](references/principles.md) for the full DDD
principles and design checklist.

See [references/ddd-chapters.md](references/ddd-chapters.md) for chapter
summaries from *Domain-Driven Design* (Evans), which is the primary source
for the principles in this skill.
