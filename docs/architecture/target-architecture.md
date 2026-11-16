# Target Architecture — Stage 1 Baseline Reproduction

**Status:** Initial Draft  
**Architecture stage:** Stage 1 — Baseline Reproduction  
**Reference:** `architecture-overview.md`  
**Decision authority:** Human-led, with AI-assisted analysis

## 1. Purpose

This document defines the initial target architecture for the Agentic Engineering Lab blog application.

The objective is to rebuild the existing application using an AI-first engineering workflow while preserving its essential architectural characteristics.

The target architecture is intentionally aligned with the legacy application. Improvements will be evaluated separately during Stage 2.

## 2. Architectural Principles

The initial implementation follows these principles:

- **Microservices:** Separate services have distinct responsibilities.
- **Separation of concerns:** Posts, comments, moderation and querying remain independent responsibilities.
- **Event-driven communication:** Services communicate relevant state changes through events.
- **Data ownership:** Business services own their respective resources.
- **Read model separation:** The Query Service provides a combined read-oriented view.
- **Baseline preservation:** Avoid unnecessary changes to the original architecture.
- **Incremental implementation:** Build and validate the system in small steps.

## 3. Target System Components

The target system contains six application components:

| Component | Type | Responsibility |
|---|---|---|
| React Client | Frontend | User interface and HTTP communication |
| Posts Service | Backend | Post creation and retrieval |
| Comments Service | Backend | Comment creation and moderation state |
| Moderation Service | Backend | Comment evaluation and moderation outcomes |
| Query Service | Backend | Combined read model |
| Event Bus | Backend | Event storage and distribution |

The five backend services retain the boundaries identified in the legacy application.

## 4. Component Responsibilities

### React Client

- Display available posts.
- Display comments associated with posts.
- Allow users to create posts.
- Allow users to submit comments.
- Retrieve information through the Query Service.

### Posts Service

- Own post data.
- Create posts.
- Retrieve posts.
- Publish `PostCreated`.

### Comments Service

- Own comment data.
- Create comments associated with posts.
- Initially assign pending moderation status.
- Process moderation outcomes.
- Publish `CommentCreated` and `CommentUpdated`.

### Moderation Service

- Receive `CommentCreated`.
- Evaluate submitted comments.
- Produce approved or rejected outcomes.
- Publish `CommentModerated`.

### Query Service

- Maintain a combined read model.
- Process post and comment events.
- Provide information to the frontend.
- Reconstruct its in-memory state using available event history during startup.

### Event Bus

- Receive events.
- Maintain an in-memory event history.
- Forward events to interested services through HTTP requests.
- Expose event history for consumers that need to reconstruct their state.

## 5. Communication Model

The architecture uses two communication patterns.

### Synchronous communication

HTTP requests are used for frontend-to-backend interactions and for event delivery between services.

The frontend retrieves posts and comments through the Query Service.

Post and comment creation requests are handled by their respective business services.

### Event-driven communication

Services publish events when relevant state changes occur.

The Event Bus distributes these events to interested consumers.

The initial event contracts are:

| Event | Publisher | Consumers |
|---|---|---|
| `PostCreated` | Posts Service | Query Service |
| `CommentCreated` | Comments Service | Query Service, Moderation Service |
| `CommentModerated` | Moderation Service | Comments Service |
| `CommentUpdated` | Comments Service | Query Service |

Event names and payload structures should remain compatible with the behavior identified in the legacy application unless a deliberate decision documents a change.

## 6. Data Ownership and Storage

For Stage 1, data storage remains in memory.

| Component | Data responsibility |
|---|---|
| Posts Service | Posts |
| Comments Service | Comments and moderation state |
| Query Service | Combined read projection |
| Event Bus | Event history |

The Query Service maintains a derived representation of information owned by the business services.

The Event Bus maintains event history to support event consumers and read-model reconstruction.

No persistent database or durable messaging infrastructure will be introduced during baseline reproduction.

## 7. Deployment and Runtime Boundaries

The initial implementation should preserve the independently deployable service structure.

The existing application uses Docker and Kubernetes manifests. The exact local development and deployment workflow for the rebuilt application will be defined during implementation planning.

The initial architecture does not prescribe additional infrastructure or tooling beyond what is needed to reproduce the baseline.

## 8. Explicitly Deferred Improvements

The following items are outside the Stage 1 architecture scope:

- Persistent databases.
- Durable message brokers.
- Advanced event delivery guarantees.
- Retry and dead-letter mechanisms.
- Advanced observability platforms.
- Authentication and authorization.
- Additional product capabilities.
- Architectural consolidation or replacement of existing services.

These improvements can be evaluated during Stage 2.

## 9. Architecture Validation

The target architecture will be considered sufficiently validated for implementation planning when:

- All six application components have clear responsibilities.
- The five backend service boundaries are understood.
- Communication patterns are documented.
- Event publishers and consumers are identified.
- Data ownership is clear.
- The target architecture remains consistent with the approved requirements and legacy baseline.

## 10. Next Steps

Use this target architecture as the technical reference for:

1. Architecture Decision Records.
2. Service implementation boundaries.
3. Incremental implementation planning.
4. Architecture and behavior validation during development.