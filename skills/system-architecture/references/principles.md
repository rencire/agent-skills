# System Architecture Principles

Distilled from *Fundamentals of Software Architecture* (Richards & Ford).
See [fosa-chapters.md](fosa-chapters.md) for the chapter summaries behind
these heuristics.

## The Two Laws

1. **Everything is a trade-off.** There is no universally correct architecture.
   Every structural decision gains something and gives something up. Name both.
2. **Why is more important than how.** The reasoning behind a decision — the
   context, the constraints, the alternatives rejected — is more valuable than
   the decision itself. Future architects need the why to know whether a decision
   still applies.

## Architecture Characteristics

The "-ilities" are not features — they are structural constraints derived from
the domain, the stakeholders, and operational reality.

- Identify at most three to five that truly constrain the design
- Implicit characteristics (security, reliability) are assumed even when unstated
- Conflicting characteristics (security vs. performance, scalability vs. simplicity) are the real design problem — resolve them explicitly
- A characteristic that cannot be measured cannot be governed

## Modularity

- **Cohesion**: things inside a module change together and belong together
- **Coupling**: modules depend on each other as little as possible
- **Connascence**: what must change together when one thing changes — minimize its scope and strength
- **Instability metric**: efferent coupling / (afferent + efferent) — higher is less stable
- Stable modules should be depended upon; unstable modules should depend outward
- Domain partitioning (by business capability) is more durable than technical partitioning (by layer)

## Architecture Quantum

The smallest independently deployable unit with high functional cohesion and
its own architecture characteristics. Use it to reason about deployment
boundaries:

- A monolith has one large quantum
- Service-based has a small number of coarse quanta
- Microservices have many fine quanta
- Design the quantum size to match team autonomy and deployment frequency needs

## Choosing an Architectural Style

1. List the top characteristics the system must support
2. Note which are in conflict
3. Sketch at least two styles and compare their characteristic trade-offs
4. Choose the style that meets must-haves at the lowest structural cost
5. Default to the simpler style unless there is clear evidence it cannot work

**Distributed is not a default.** Distributed systems gain scalability and
deployability but pay in operational complexity, network latency, partial
failure, and data consistency. Choose distribution when independent
deployability is a genuine top-tier requirement, not as a default or
because it feels modern.

## Style Trade-offs Summary

- **Layered monolith**: low complexity, low deployability, poor scalability — start here
- **Modular monolith**: better domain isolation, still one deployment unit — grow here
- **Service-based**: coarse-grained services, shared database — pragmatic distribution step
- **Event-driven (broker)**: high decoupling, poor traceability — async workflows
- **Event-driven (mediator)**: central orchestration, easier to trace — complex workflows
- **Microservices**: maximum deployability, maximum operational cost — only when justified
- **Microkernel**: plug-in extensibility — platform products
- **Pipeline**: composable transforms, stateless filters — batch/ETL

## Fitness Functions

Characteristics erode without governance. Automate their verification:

- Dependency direction: domain modules must not import infrastructure
- Coupling metrics: flag modules that exceed a coupling threshold
- Performance budgets: latency and throughput assertions in CI
- Security: static analysis and known-vulnerability scans
- Cycle detection: no circular dependencies between bounded contexts

## Architecture Decision Records

Record significant structural decisions while the reasoning is fresh:

- **Context**: what constraint or situation led here?
- **Decision**: what was chosen?
- **Consequences**: what does this enable, and what does it make harder?
- **Alternatives considered**: what was rejected and why?

The record of alternatives rejected is often the most valuable part.

## Architecture Review Checklist

1. What are the top three to five architecture characteristics this system must support?
2. Which characteristics are in conflict, and how is that conflict resolved?
3. What architectural style is being used, and does it optimize for the right characteristics?
4. What is the architecture quantum — the smallest independently deployable unit?
5. Is the domain partitioned by capability, or by technical layer?
6. Are fitness functions in place to govern the key characteristics over time?
7. Is the rationale for this decision recorded in an ADR?
8. Is distribution justified, or is a monolith being dismissed prematurely?
