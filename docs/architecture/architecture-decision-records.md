# Architecture Decision Records (ADRs)

**Status:** Initial Draft  
**Architecture stage:** Stage 1 — Baseline Reproduction

## 1. Purpose

Architecture Decision Records document significant technical decisions made during the development of the Agentic Engineering Lab application.

They provide a historical record of:

- The context behind a decision.
- The problem or question being addressed.
- The alternatives considered.
- The selected approach.
- The consequences and trade-offs.

Decisions should remain understandable even when the implementation evolves.

## 2. Decision Status Convention

| Status | Meaning |
|---|---|
| Proposed | Under discussion; not yet approved |
| Accepted | Approved for implementation |
| Rejected | Considered but not selected |
| Superseded | Replaced by a later decision |

## 3. ADR-001: Preserve the Microservices Architecture

**Status:** Accepted

### Context

The existing blog application is organized around separate services with distinct responsibilities.

These services include Posts, Comments, Moderation, Query and Event Bus.

The experiment aims to reproduce the existing architecture before evaluating architectural improvements.

### Decision

Preserve the five backend service boundaries:

- Posts Service.
- Comments Service.
- Moderation Service.
- Query Service.
- Event Bus.

The React Client remains a separate frontend component.

### Alternatives Considered

- Consolidate backend responsibilities into a single application.
- Redesign service boundaries before implementation.
- Preserve the existing service boundaries.

### Rationale

Preserving the existing architecture creates a controlled baseline for comparing traditional and AI-assisted development approaches.

### Consequences

- Multiple services must be developed and integrated.
- Service responsibilities must remain clearly defined.
- Local development requires coordinating multiple components.
- Architectural consolidation is deferred to a future evaluation.

## 4. ADR-002: Preserve Event-Driven Communication

**Status:** Accepted

### Context

The existing application uses a dedicated Event Bus to distribute state-change events between services.

The Query Service consumes events to maintain its read model, while the Moderation Service evaluates submitted comments.

### Decision

Preserve event-driven communication using the existing Event Bus pattern.

The initial event types are:

- `PostCreated`
- `CommentCreated`
- `CommentModerated`
- `CommentUpdated`

HTTP requests remain the transport mechanism used by the current implementation.

### Alternatives Considered

- Direct synchronous communication between all backend services.
- Introduce a dedicated external message broker.
- Preserve the existing HTTP-based Event Bus.

### Rationale

The current event-driven approach is a core characteristic of the original system and should remain part of the initial rebuild.

### Consequences

- Event contracts must be understood and maintained.
- Consumers must process relevant events.
- Event delivery and failure behavior require careful validation.
- More advanced messaging infrastructure is deferred.

## 5. ADR-003: Use In-Memory Storage for Stage 1

**Status:** Accepted

### Context

The existing implementation stores posts, comments and event history in memory.

Although persistent storage would be more appropriate for a production system, introducing databases would change the baseline architecture.

### Decision

Preserve in-memory storage for Stage 1.

Each service maintains the data associated with its responsibility.

### Alternatives Considered

- Introduce a relational database.
- Introduce a document database.
- Preserve in-memory storage.

### Rationale

In-memory storage keeps the initial rebuild aligned with the existing implementation and allows us to focus on service boundaries, event communication and AI-first development practices.

### Consequences

- Data is lost when services restart.
- Event history is not durable.
- Read-model reconstruction depends on available event history.
- Persistent storage will be evaluated in Stage 2.

## 6. ADR-004: Separate Baseline Reproduction from Architecture Evolution

**Status:** Accepted

### Context

The existing application has limitations involving persistence, event delivery, resilience and observability.

Addressing all these limitations during the initial rebuild would make it difficult to compare the original system with the new implementation.

### Decision

Organize architecture work into two stages.

**Stage 1 — Baseline Reproduction**

Rebuild the existing application while preserving its essential architecture and behavior.

**Stage 2 — Architecture Evolution**

Evaluate and introduce improvements through separate, documented architecture decisions.

### Alternatives Considered

- Improve the architecture during the initial rebuild.
- Reproduce the baseline first and evolve it afterward.

### Rationale

Separating the stages provides a controlled learning environment and makes architectural changes explicit.

### Consequences

- Some known limitations will intentionally remain during Stage 1.
- Stage 2 improvements must be prioritized and justified.
- Baseline behavior should be validated before evolution begins.

## 7. Future Architecture Decisions

The following topics may require additional ADRs as the project evolves:

| Topic | Potential decision |
|---|---|
| Persistence | Database technology and data ownership |
| Messaging | Event durability and delivery guarantees |
| Resilience | Retry, timeout and failure-handling strategy |
| Observability | Logging, metrics and tracing |
| Configuration | Environment and service configuration |
| Security | Authentication and authorization |
| Deployment | Container orchestration and delivery approach |

These are future topics, not approved implementation decisions.

## 8. Decision Management

When a significant architecture decision is required:

1. Describe the context and problem.
2. Identify viable alternatives.
3. Evaluate trade-offs.
4. Recommend an approach.
5. Obtain human approval.
6. Record the decision and its consequences.
7. Update the relevant architecture documentation.

AI assistants can help investigate alternatives and explain trade-offs, but they must not independently approve significant architecture decisions.