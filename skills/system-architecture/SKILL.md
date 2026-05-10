---
name: system-architecture
description: Use when making system-level structural decisions: choosing an architectural style, defining service boundaries, evaluating architecture characteristics (scalability, deployability, reliability), or documenting architecture decisions. For code and module level design, use the software-architecture skill instead.
---

# System Architecture

Use this skill for system-level design decisions: architectural style selection,
service boundary definition, architecture characteristic trade-offs, and
decision documentation. For code and module level design, use the
`software-architecture` skill instead.

## Default Lens

- Start from architecture characteristics, not preferred styles.
- Everything is a trade-off — name what is being gained and what is being given up.
- The *why* behind a decision is more valuable than the decision itself.
- Default to the simplest style that meets the required characteristics.
- Distributed systems are not a default — they are chosen when independent deployability is a genuine top-tier requirement.

## Decision Workflow

1. Identify the top three to five architecture characteristics the system must support (from domain requirements and operational constraints).
2. Note which characteristics are in conflict — those conflicts are the real design problem.
3. Sketch at least two architectural approaches and compare their characteristic trade-offs explicitly.
4. Choose the style that satisfies the must-have characteristics at the lowest structural cost.
5. Document the decision as an ADR: context, decision, consequences, alternatives considered.

## Architecture Characteristics

Derive these from stakeholders and domain requirements, not from preference:

- **Scalability** — handles growing load by adding resources
- **Elasticity** — scales up and down rapidly in response to bursts
- **Deployability** — how easily and safely individual pieces can be released
- **Reliability / Availability** — tolerates failures and stays operational
- **Performance** — responds within required latency bounds
- **Security** — controls access and protects data
- **Testability** — can be verified in isolation
- **Observability** — internal state is visible and traceable

Never optimize for all characteristics simultaneously. Pick the ones that most constrain the design and accept the trade-offs on the rest.

## Architectural Styles Cheat Sheet

| Style | Optimize For | Cost | Use When |
|---|---|---|---|
| Layered monolith | Simplicity, speed to start | Scalability, deployability | Small team, simple domain, internal tooling |
| Modular monolith | Domain isolation without distribution | Slightly more upfront design | Growing team, clear domains, before going distributed |
| Service-based | Deployability, domain isolation | Shared database limits data isolation | Mid-size team moving off a monolith |
| Event-driven | Decoupling, async scalability | Traceability, ordering, error handling | High-volume async workflows, loose coupling needed |
| Microservices | Independent deployability, team autonomy | Operational complexity, data consistency | Large org, truly independent teams, high deployability is critical |
| Microkernel | Extensibility, plug-in ecosystem | Plug-in contract complexity | Platform products with third-party extensions |
| Pipeline | Throughput, composable transforms | Not suited for interactive use | ETL, batch processing, data transformation |

## Modularity Heuristics

- **High cohesion**: things inside a module are strongly related
- **Low coupling**: modules depend on each other as little as possible
- **Domain partitioning** (split by business capability) scales better than technical partitioning (split by layer) as the team grows
- The **architecture quantum** is the smallest unit that is independently deployable with its own characteristics — design around it
- Afferent coupling (others depend on you) signals a stable module; efferent coupling (you depend on others) signals instability

## Fitness Functions

Encode architecture constraints as automated checks so they survive over time:

- Dependency direction tests (nothing in domain should import infra)
- Coupling metrics thresholds
- Performance budgets in CI
- Security and compliance scans
- Cycle detection between modules

## Architecture Decision Records

Every significant structural decision should be recorded:

```
# ADR-NNN: [Short title]

## Context
What situation or constraint led to this decision?

## Decision
What was decided?

## Consequences
What does this enable? What does it make harder?

## Alternatives Considered
What else was evaluated and why was it not chosen?
```

## Relationship to Domain-Driven Design

Bounded contexts from DDD are the natural unit for service decomposition.
Before deciding on an architectural style, use the `domain-driven-design` skill
to identify bounded contexts and draw the context map. Then use this skill to
decide how those contexts map to deployment units:

- A single bounded context can be a module in a monolith, a service in a
  service-based architecture, or a microservice — the choice is a system
  architecture decision driven by the deployment and team characteristics, not
  the domain model itself.
- The context map integration patterns (ACL, shared kernel, customer/supplier)
  constrain which architectural styles are viable — a conformist relationship
  rarely justifies a separate microservice.
- High coupling between bounded contexts is a signal to reconsider the context
  boundaries before choosing a distributed style.

## When To Escalate Complexity

- Moving from monolith to distributed: only when independent deployability is actively blocking delivery
- Splitting a service: only when the service has genuinely independent teams or deployment lifecycles
- Adding a message bus: only when direct coupling is creating a measurable problem

## Reference

See [references/principles.md](references/principles.md) for the full
system architecture checklist.

See [references/fosa-chapters.md](references/fosa-chapters.md) for chapter
summaries from *Fundamentals of Software Architecture* (Richards & Ford),
which is the primary source for the principles in this skill.
