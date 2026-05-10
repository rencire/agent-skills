# Fundamentals of Software Architecture — Chapter Summaries

Distilled from *Fundamentals of Software Architecture* by Mark Richards and Neal Ford.
These summaries capture the intent of each chapter as practical heuristics,
not as a reproduction of the original text.

---

## Ch 1 — Introduction

Software architecture sits at the intersection of structure, characteristics,
decisions, and principles. Architects think in terms of trade-offs, not solutions.
The First Law: *everything in software architecture is a trade-off*. The Second
Law: *why is more important than how* — the reasoning behind a decision matters
more than the decision itself.

## Ch 2 — Architectural Thinking

Architects must think differently from developers: wider scope, longer time
horizon, and explicit attention to trade-offs. The architect's job is to
understand what the business needs and translate that into structural constraints.
Maintaining technical depth while gaining breadth is the defining challenge of
the role.

## Ch 3 — Modularity

Modularity is the default mechanism for managing complexity at scale. Key
metrics: **cohesion** (how related are the things inside a module?),
**coupling** (how dependent are modules on each other?), and **connascence**
(what must change together when one thing changes?). High cohesion and low
coupling are the twin goals. Afferent coupling (incoming) signals stability;
efferent coupling (outgoing) signals instability. Instability = efferent /
(afferent + efferent).

## Ch 4 — Architecture Characteristics Defined

Architecture characteristics (formerly "non-functional requirements") are the
"-ilities" that shape structural decisions: scalability, elasticity,
reliability, availability, performance, security, testability, deployability,
observability. They are not features — they are constraints on how the system
must behave. Implicit characteristics (security, reliability) are assumed even
when not stated.

## Ch 5 — Identifying Architecture Characteristics

Derive characteristics from: domain requirements, stakeholder concerns, and
known operational constraints. Don't try to optimize for everything — pick the
three to five that most constrain the design. Characteristics in conflict (e.g.,
security vs. performance) force explicit trade-off decisions.

## Ch 6 — Measuring and Governing Architecture Characteristics

Characteristics must be measurable to be actionable. **Fitness functions** are
automated checks that verify a characteristic holds over time: dependency
direction tests, performance budgets, coupling metrics, security scans. Without
governance, characteristics erode as the system evolves.

## Ch 7 — Scope of Architecture Characteristics

Architecture characteristics apply at different scopes: the whole system, a
service, or a component. The **architecture quantum** is the smallest deployable
unit that has high functional cohesion, independent deployability, and its own
set of architectural characteristics. Microservices have many small quanta;
monoliths have one large one.

## Ch 8 — Component-Based Thinking

Components are the building blocks of architecture. Define component boundaries
before choosing an architectural style. Two approaches: **technical partitioning**
(split by layer: presentation, business, persistence) and **domain partitioning**
(split by business capability). Domain partitioning aligns better with how
systems change and scales better for teams.

## Ch 9 — Architecture Styles: Foundations

An architectural style is a named, repeatable organizational structure with known
trade-offs. No style is universally best. Choosing a style means choosing which
characteristics to optimize and which to sacrifice. The important distinction:
**monolithic** (single deployable unit) vs **distributed** (multiple deployable
units). Distributed systems gain scalability and deployability but pay in
complexity, latency, and reliability surface area (the "fallacies of distributed
computing").

## Ch 10 — Layered Architecture

The default starting point. Layers: presentation, business, persistence, database.
Simple, well-understood, and easy to get started with. Weaknesses: changes
often cut across all layers; low deployability; does not scale well; encourages
"architecture by accident". Good for small teams, simple domains, and existing
systems. Avoid if the domain is complex or the team is large.

## Ch 11 — Pipeline Architecture

Data flows through a series of independent, stateless filters connected by
pipes. Each filter transforms the data and passes it on. Excellent for batch
processing, ETL, and data transformation workflows. Not suited for interactive
or request/response systems.

## Ch 12 — Microkernel Architecture

A small core system (the kernel) with plug-in modules that extend it. Common in
IDEs, build tools, and product platforms that support third-party extensions.
Good for systems that need to support many variations of behavior. Complexity
lives in the plug-in contract and registry.

## Ch 13 — Service-Based Architecture

A pragmatic middle ground between monolith and microservices. Coarse-grained
services (4–12) that share a database. Lower operational complexity than
microservices. Good deployability and domain isolation without the full cost of
independent data stores. Suitable for most mid-size teams moving away from a
monolith.

## Ch 14 — Event-Driven Architecture

Components communicate through events rather than direct calls. Two topologies:
**broker** (components publish and subscribe without a central coordinator) and
**mediator** (a central orchestrator routes events). High scalability and
decoupling, but harder to trace, debug, and reason about. Error handling and
event ordering are significant design challenges.

## Ch 15 — Space-Based Architecture

Designed to eliminate database bottlenecks at extreme scale. Uses in-memory
data grids and replicated processing units. Complex and expensive to operate.
Reserved for systems with genuinely massive and unpredictable load (ticket
sales, auction systems).

## Ch 16 — Orchestration-Driven SOA

The classical enterprise SOA style: a central enterprise service bus (ESB)
orchestrates many fine-grained services. Largely considered a failed style due
to tight coupling through the ESB, high operational cost, and poor change
agility. Included for historical context and to understand what microservices
are reacting against.

## Ch 17 — Microservices Architecture

Each service owns its own data store and is independently deployable. Optimizes
for high deployability, scalability, and team autonomy. Pays a steep cost in
operational complexity, distributed system problems (network latency, partial
failure, data consistency), and service granularity decisions. Not a default
choice — only appropriate when independent deployability is a genuine top-tier
requirement.

## Ch 18 — Choosing the Appropriate Architecture Style

Start from the architecture characteristics the system must support, not from
a preferred style. Then consider: team size and distribution, domain complexity,
data requirements, and operational maturity. A large team with a complex domain
and high deployability needs may warrant microservices. A small team building an
internal tool does not. The layered monolith or modular monolith should be the
default until there is clear evidence it cannot meet requirements.

## Ch 19 — Architecture Decisions

Architecture decisions are the structural choices that are hard to reverse and
affect the whole system. Document them with **Architecture Decision Records
(ADRs)**: the context, the decision, the consequences, and the alternatives
considered. The record of *why* is more valuable than the decision itself —
future architects need to know what was traded away, not just what was chosen.

## Ch 20 — Analyzing Architecture Risk

Identify risks by assessing the impact and likelihood of failure for each
structural decision. Maintain a risk matrix. High-impact, high-likelihood risks
must be mitigated architecturally, not just operationally. Risk analysis is a
recurring activity, not a one-time gate.

## Ch 21 — Diagramming and Presenting Architecture

Architectural diagrams communicate structure to different audiences. The C4 model
(Context, Container, Component, Code) provides a useful set of zoom levels.
Keep diagrams at the right level of abstraction for the audience. Avoid
whiteboard diagrams that mix deployment topology with logical structure.

## Ch 22–24 — Teams, Leadership, and Career

Effective architects collaborate across team boundaries, communicate trade-offs
in business terms, and build trust by being technically credible. The architect's
influence is usually through persuasion, not authority. Career growth means
expanding scope while maintaining enough technical depth to be trusted.
