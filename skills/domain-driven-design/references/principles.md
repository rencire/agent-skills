# Domain-Driven Design Principles

Distilled from Eric Evans' *Domain-Driven Design*. See
[ddd-chapters.md](ddd-chapters.md) for the chapter summaries behind these
heuristics.

## Foundation

- The domain model is the shared understanding between developers and domain
  experts — it is not a diagram, a document, or a database schema. It lives
  in the code.
- The **Ubiquitous Language** is the single vocabulary used by everyone on the
  team: in conversations, in tickets, in tests, and in code. Divergence between
  the language in the code and the language domain experts use is a model defect.
- Modeling is knowledge crunching: an iterative process of extracting, questioning,
  and distilling domain insight until the model reflects how the domain actually
  works.

## Building Blocks

### Entities

- Defined by identity, not attributes
- Two entities with the same attributes are still distinct objects
- Identity must be meaningful in the domain, not just a database key
- Minimize mutable state; only change what the domain says can change

### Value Objects

- Defined entirely by their attributes; no identity
- Two value objects with the same attributes are interchangeable
- Should be immutable — replace, do not mutate
- Prefer value objects over entities when identity does not matter
- Good candidates: measurements, money, dates, coordinates, descriptions

### Domain Services

- Stateless operations that belong to the domain but do not fit on any entity
  or value object
- Named using Ubiquitous Language verbs
- Should be rare — most domain logic belongs on entities or value objects
- Do not confuse with application services (which orchestrate use cases) or
  infrastructure services (which handle I/O)

### Aggregates

- A cluster of entities and value objects with a single **Aggregate Root**
- External objects hold references only to the root, never to internal members
- The aggregate is the consistency boundary — all invariants are enforced
  within one aggregate in a single transaction
- Keep aggregates small; large aggregates are a sign of missing boundaries
- Reference other aggregates by identity, not by object reference

### Factories

- Encapsulate the complex creation logic for aggregates and entities
- Callers should not need to know how an object is assembled
- Use when the constructor would require too much domain knowledge to call correctly

### Repositories

- One repository per aggregate root
- Provide the illusion of an in-memory collection; abstract all persistence
- The domain layer should not know about databases, ORMs, or query languages
- Repositories belong to the domain layer but are implemented in the infrastructure layer

## Layered Architecture

- **Domain layer**: entities, value objects, aggregates, domain services —
  pure domain logic, no infrastructure dependencies
- **Application layer**: orchestrates use cases, calls repositories and domain
  services, handles transactions — no business rules
- **Infrastructure layer**: persistence, messaging, external APIs — implements
  interfaces defined by the domain
- **UI layer**: presentation and input — delegates to application layer

Domain logic that leaks into the application or UI layer becomes untestable,
duplicated, and hard to change.

## Strategic Design

### Bounded Contexts

- A bounded context is an explicit boundary within which a single model applies
  and the Ubiquitous Language is consistent
- The same term can mean different things in different bounded contexts — that
  is acceptable; ambiguity within a context is not
- Name each bounded context explicitly and enforce its boundaries in code

### Context Map

- Document the relationships between bounded contexts explicitly
- Choose the right integration pattern for each relationship:
  - **Anticorruption Layer**: protect your model from an upstream model you
    don't control; translate at the boundary
  - **Shared Kernel**: shared model subset — small, stable, and coordinated
  - **Customer/Supplier**: downstream negotiates requirements with upstream
  - **Conformist**: adopt the upstream model wholesale when negotiation is not
    possible and the cost of an ACL is not worth it
  - **Open Host Service / Published Language**: stable, documented protocol for
    many consumers

### Core Domain

- The Core Domain is the part of the system that is central to the business's
  competitive advantage
- Invest the most design effort here; do not let it be crowded out by
  supporting concerns
- Supporting subdomains: necessary but not differentiating — keep them simple
- Generic subdomains: commodity capabilities — buy, use off-the-shelf, or build
  with minimal investment

## Refactoring Toward Deeper Insight

- Continuously refactor to reflect new domain understanding in the code
- Listen for friction: complex algorithms often hide an implicit domain concept
  waiting to be named
- When you find an implicit concept, give it a name from the Ubiquitous Language
  and make it explicit in the model
- Watch for breakthroughs: a reframing that suddenly makes many difficult things
  simple — act on them quickly even if the refactoring is large

## Design Review Checklist

1. Is the Ubiquitous Language used consistently in code, tests, and conversation?
2. Is each class clearly an entity, value object, or domain service?
3. Are aggregate boundaries drawn at the consistency boundary, not the
   convenience boundary?
4. Do external objects hold references only to aggregate roots?
5. Is the domain layer free of infrastructure and application concerns?
6. Are bounded contexts named and their boundaries enforced?
7. Is the Core Domain identified and receiving proportionate design investment?
8. Are cross-context integrations using the right pattern (ACL, shared kernel, etc.)?
9. Are there implicit domain concepts hiding in complex code that should be
   named and extracted?

## Naming Heuristics

- Entity names: nouns that are meaningful in the domain (`Order`, `Customer`,
  `Shipment`)
- Value object names: descriptive nouns that convey their meaning completely
  (`Money`, `Address`, `DateRange`)
- Domain service names: verb phrases that describe the operation
  (`FundsTransferService`, `RouteCalculator`)
- Repository names: `[AggregateName]Repository` — one per aggregate root
- Factory names: `[AggregateName]Factory` or a factory method on the root
- Bounded context names: use the language the domain experts use for that area
  of the business
