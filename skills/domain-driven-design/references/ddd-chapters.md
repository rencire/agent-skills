# Domain-Driven Design — Chapter Summaries

Distilled from Eric Evans' *Domain-Driven Design: Tackling Complexity in the
Heart of Software*. These summaries capture the intent of each chapter as
practical heuristics, not as a reproduction of the original text.

---

## Part I — Putting the Domain Model to Work

### Ch 1 — Crunching Knowledge

Effective software requires deep collaboration between developers and domain
experts. The developer's job is not to transcribe requirements but to actively
model the domain — extracting, questioning, and distilling knowledge until the
model reflects how the domain actually works. This process is iterative and
never fully complete. A model that is not continuously refined against real
domain insight will drift from reality and become a liability.

### Ch 2 — Communication and the Use of Language

A shared language between developers and domain experts — the **Ubiquitous
Language** — is the foundation of a useful model. This language must be used
consistently in conversations, documents, and code. When the language in the
code diverges from the language domain experts use, the model is lying. The
Ubiquitous Language is not a translation layer; it is the single vocabulary
everyone uses.

### Ch 3 — Binding Model and Implementation

The model and the implementation must remain synchronized. A model that exists
only in documents or heads is not a domain model — it is a diagram. The code
must directly express the model, and changes to the model must be reflected
immediately in the code. This binding is what makes the model useful and
testable. Avoid a "design layer" that is separate from the code that runs.

---

## Part II — Building Blocks of a Model-Driven Design

### Ch 4 — Isolating the Domain

Domain logic must be separated from infrastructure, UI, and application concerns.
The **Layered Architecture** achieves this: UI, application, domain, and
infrastructure layers, each with a clear responsibility. The domain layer
contains the model and the business rules — nothing else. When domain logic
leaks into the application layer or the UI, it becomes untestable, duplicated,
and hard to change.

### Ch 5 — A Model Expressed in Software

Three core building blocks for expressing the model:

- **Entities**: objects defined by a continuous identity that persists across
  state changes. Two entities with the same attributes are still distinct if
  they have different identities (e.g., two people with the same name).
- **Value Objects**: objects defined entirely by their attributes. Two value
  objects with the same attributes are interchangeable. They should be
  immutable. Prefer value objects over entities when identity does not matter.
- **Domain Services**: stateless operations that belong to the domain but do
  not naturally fit on any entity or value object (e.g., a transfer between
  two accounts).

### Ch 6 — The Life Cycle of a Domain Object

Three patterns for managing the lifecycle of domain objects:

- **Aggregates**: a cluster of entities and value objects with a single root
  entity (the **Aggregate Root**). External objects may only hold references
  to the root, not to internal members. The aggregate defines the consistency
  boundary — invariants are enforced within one aggregate.
- **Factories**: encapsulate the complex logic of creating aggregates or
  entities. Callers should not need to know how an object is assembled.
- **Repositories**: provide the illusion of an in-memory collection of
  aggregates. One repository per aggregate root. Repositories abstract
  persistence — the domain should not know about databases.

### Ch 7 — Using the Language: An Extended Example

A worked example showing how Ubiquitous Language, entities, value objects,
aggregates, and repositories come together in a realistic domain model. The
lesson: the model is not found in one step; it emerges through iteration,
naming, and constant pressure to align code with domain insight.

---

## Part III — Refactoring Toward Deeper Insight

### Ch 8 — Breakthrough

Models improve gradually, but occasionally a team reaches a **breakthrough**:
a reframing of the model that suddenly makes many previously difficult things
simple. Breakthroughs are disruptive (they require significant refactoring)
but are the payoff of sustained strategic investment in the model. Recognize
them when they arrive and act on them quickly.

### Ch 9 — Making Implicit Concepts Explicit

The biggest leaps in model quality come from surfacing concepts that were
previously implicit — hiding in code comments, in the heads of domain experts,
or in the gaps between procedures. When you find yourself writing a complex
algorithm, ask: is there a domain concept hiding here that should have a name?
Specifications, constraints, and processes are often implicit concepts worth
making explicit.

### Ch 10 — Supple Design

A supple design makes the model easy to use correctly and hard to use
incorrectly. Key patterns:

- **Intention-Revealing Interfaces**: names describe what, not how
- **Side-Effect-Free Functions**: queries return results without changing state
- **Assertions**: state post-conditions and invariants explicitly
- **Conceptual Contours**: break down operations at natural domain boundaries
- **Standalone Classes**: minimize dependencies to make classes understandable in isolation
- **Closure of Operations**: operations that return the same type they operate on (e.g., adding two amounts returns an amount)

### Ch 11 — Applying Analysis Patterns

Analysis patterns are reusable conceptual structures for recurring domain
problems (from Martin Fowler's *Analysis Patterns*). They are starting points
for modeling, not direct implementations. Use them to accelerate knowledge
crunching when the domain resembles a known pattern (accounting, measurements,
knowledge representation). Adapt them to the Ubiquitous Language — do not
import their original vocabulary wholesale.

### Ch 12 — Relating Design Patterns to the Model

Gang of Four design patterns (Strategy, Composite, etc.) can be expressed as
domain model elements when the pattern reflects a real domain concept. A
Strategy that encodes a business rule is a domain object, not just a
refactoring technique. The test: does the pattern have a name in the
Ubiquitous Language? If so, it belongs in the domain model. If not, it is
an implementation detail.

### Ch 13 — Refactoring Toward Deeper Insight

Continuous refactoring is not just about clean code — it is the mechanism for
improving the model. Each refactoring is an opportunity to reflect new domain
understanding in the code. The process: listen for friction, find the implicit
concept, give it a name from the Ubiquitous Language, refactor the code to
express it, validate with domain experts. Repeat.

---

## Part IV — Strategic Design

### Ch 14 — Maintaining Model Integrity

Large systems cannot have a single unified model. Instead, the system is divided
into **Bounded Contexts** — explicit boundaries within which a model is
consistent and the Ubiquitous Language applies. The same term can mean different
things in different bounded contexts; that is acceptable as long as each context
is internally consistent. A **Context Map** documents the relationships between
bounded contexts.

Integration patterns between bounded contexts:

- **Shared Kernel**: two contexts share a subset of the model; changes require
  coordination
- **Customer/Supplier**: upstream context produces for a downstream consumer;
  the downstream negotiates requirements
- **Conformist**: downstream adopts the upstream model wholesale, with no
  negotiation
- **Anticorruption Layer**: downstream translates the upstream model into its
  own through a translation layer, protecting its model from upstream changes
- **Separate Ways**: contexts have no integration; teams work independently
- **Open Host Service**: upstream publishes a stable protocol for many
  downstream consumers
- **Published Language**: a well-documented shared language for inter-context
  communication

### Ch 15 — Distillation

In a large model, not everything is equally important. The **Core Domain** is
the part of the model that is central to the business's competitive advantage —
the part worth the most investment. Supporting subdomains and generic subdomains
(commodity capabilities) should be kept simple or bought off the shelf. The
goal is to concentrate design effort on the Core Domain and not let it be
crowded out by supporting concerns.

### Ch 16 — Large-Scale Structure

When a system is large enough, local model decisions need to fit into a
larger organizing pattern — a **Large-Scale Structure** that guides how modules
and bounded contexts relate. Examples: layered systems, responsibility layers,
knowledge levels. The structure should evolve with the model, not be imposed
upfront. A structure that constrains the model more than it guides it should
be abandoned.

### Ch 17 — Bringing the Strategy Together

Strategic design decisions — bounded contexts, context maps, core domain
distillation, large-scale structure — are not made once and forgotten. They
evolve as the team's understanding deepens. The key discipline: keep the Core
Domain pure, protect bounded context boundaries, and invest continuously in
making the model reflect real domain insight. Strategy and tactics reinforce
each other.
