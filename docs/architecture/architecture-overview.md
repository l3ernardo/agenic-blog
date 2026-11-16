# Architecture Overview — Existing System Baseline

**Status:** Initial Draft  
**Architecture:** Microservices and event-driven communication  
**Implementation scope:** Stage 1 — Baseline Reproduction  
**Source:** Existing Application Discovery Report and architecture review  
**Validation:** Static source inspection. Runtime behavior has not been independently verified.

## 1. Purpose

This document establishes the architecture baseline for the Agentic Engineering Lab experiment.

The objective is to understand and document the existing blog application's architecture before planning its rebuild.

The initial implementation will reproduce the existing architecture and user-facing behavior. Architectural improvements will be evaluated separately in a future evolution stage.

The experiment aims to compare traditional software development practices with an AI-first, agentic engineering approach while maintaining a consistent product and architectural baseline.

## 2. Architecture Overview

The application follows a microservices architecture organized around distinct responsibilities.

It consists of:

- A React frontend client.
- Five backend services: Posts, Comments, Moderation, Query and Event Bus.
- HTTP-based communication between the frontend and backend services.
- Event-driven communication between backend services.
- In-memory data storage in the current implementation.

The Posts and Comments services are the core business services. They own their respective resources and operations.

The Moderation Service evaluates submitted comments, while the Query Service maintains a combined read model. The Event Bus distributes events and enables services to communicate without directly depending on each other's internal implementation.

## 3. High-Level Architecture

| Layer | Component | Responsibility |
|---|---|---|
| Presentation | React Client | User interface for posts and comments |
| Business | Posts Service | Creates and manages posts |
| Business | Comments Service | Creates and manages comments |
| Processing | Moderation Service | Evaluates comments and produces moderation outcomes |
| Read Model | Query Service | Provides a combined view of posts and comments |
| Integration | Event Bus | Stores and distributes events |

### Architectural relationships

1. The React Client sends HTTP requests to backend endpoints.
2. The Posts and Comments services process operations on their respective resources.
3. When relevant changes occur, services publish events.
4. The Event Bus receives and forwards events to interested services.
5. The Moderation Service evaluates submitted comments and publishes moderation outcomes.
6. The Query Service processes events to maintain a combined read model.
7. The frontend retrieves post and comment information through the Query Service.

## 4. Backend Service Responsibilities

### 4.1 Posts Service

**Port:** 4000  
**Technology:** Node.js and Express.

The Posts Service owns the post resource.

Responsibilities:

- Create blog posts.
- Maintain posts in memory.
- Retrieve available posts.
- Publish `PostCreated` when a post is created.

Relevant endpoints:

- `GET /posts`
- `POST /posts/create`
- `POST /events`

### 4.2 Comments Service

**Port:** 4001  
**Technology:** Node.js and Express.

The Comments Service owns the comment resource and its moderation state.

Responsibilities:

- Create comments associated with posts.
- Maintain comments in memory.
- Initially assign comments a pending moderation status.
- Process moderation results.
- Publish `CommentCreated` when a comment is submitted.
- Publish `CommentUpdated` when a comment's moderation state changes.

Relevant endpoints:

- `GET /posts/:id/comments`
- `POST /posts/:id/comments`
- `POST /events`

### 4.3 Moderation Service

**Port:** 4003  
**Technology:** Node.js and Express.

The Moderation Service evaluates submitted comments and produces moderation outcomes.

Responsibilities:

- Receive comment-related events.
- Evaluate submitted comments.
- Determine whether comments are approved or rejected.
- Publish `CommentModerated` with the moderation outcome.

The current implementation rejects comments containing the lowercase substring `orange`. Other comments are approved.

This is an observed implementation rule, not a validated business moderation policy.

Although moderation could be extended to other content types in the future, the inspected implementation currently handles comments.

### 4.4 Query Service

**Port:** 4002  
**Technology:** Node.js and Express.

The Query Service provides a combined read-oriented view of posts and comments.

Responsibilities:

- Maintain a read model of posts and comments.
- Process `PostCreated`, `CommentCreated` and `CommentUpdated` events.
- Provide post and comment information to the frontend.
- Request event history during startup and replay events to reconstruct its in-memory state.

The Query Service separates read operations from the business services that own the underlying resources.

### 4.5 Event Bus

**Port:** 4005  
**Technology:** Node.js, Express and Axios.

The Event Bus provides event distribution between backend services.

Responsibilities:

- Receive events from services.
- Store events in an in-memory collection.
- Forward events to interested services through HTTP requests.
- Expose an endpoint to retrieve event history.

The Event Bus enables event-driven communication and reduces the need for services to communicate directly with every other service.

The current implementation does not wait for all subscriber requests to finish before acknowledging the original event request.

## 5. Event-Driven Communication

The application uses events to communicate relevant state changes between services.

### Main event types

| Event | Publisher | Consumers |
|---|---|---|
| `PostCreated` | Posts Service | Query Service |
| `CommentCreated` | Comments Service | Query Service, Moderation Service |
| `CommentModerated` | Moderation Service | Comments Service |
| `CommentUpdated` | Comments Service | Query Service |

### Comment moderation flow

1. A user submits a comment through the React Client.
2. The Comments Service creates the comment with pending status.
3. The Comments Service publishes `CommentCreated`.
4. The Event Bus forwards the event to the Query and Moderation services.
5. The Moderation Service evaluates the comment.
6. The Moderation Service publishes `CommentModerated`.
7. The Comments Service updates the comment's moderation status.
8. The Comments Service publishes `CommentUpdated`.
9. The Query Service updates its read model.

This flow demonstrates how event notifications coordinate changes across independently responsible services.

## 6. Data Ownership and Storage

Each business service owns the data associated with its responsibility.

| Component | Data maintained |
|---|---|
| Posts Service | Posts |
| Comments Service | Comments and moderation status |
| Query Service | Combined read model of posts and comments |
| Event Bus | Event history |
| Moderation Service | Moderation processing; no separate persistent data store identified |

The current implementation uses in-memory JavaScript objects and arrays. No persistent database or durable message broker was identified during source inspection.

For Stage 1, this storage approach will be preserved.

Known implications include:

- Data can be lost when services restart.
- Event history is not durable across Event Bus restarts.
- The Query Service depends on event history to reconstruct its read model.
- Service availability and event delivery can affect data consistency.

The possibility of introducing persistent storage will be evaluated in Stage 2.

## 7. Deployment Architecture

The repository contains individual Dockerfiles and Kubernetes manifests for application components.

Observed characteristics:

- Services are containerized individually.
- Kubernetes Deployments define service workloads.
- Kubernetes Services provide internal service access.
- NGINX Ingress exposes the application through the `posts.com` host.
- The frontend uses configured service URLs to communicate with backend endpoints.

No root-level Docker Compose configuration, CI/CD pipeline or automated image build and publishing workflow was identified during discovery.

Runtime infrastructure and deployment dependencies remain to be validated.

## 8. Architectural Constraints and Current Limitations

| Area | Current implementation |
|---|---|
| Persistence | In-memory storage |
| Event delivery | HTTP-based forwarding |
| Event durability | In-memory event history |
| Error handling | Limited visible handling of delivery failures |
| Resilience | No visible retry or timeout strategy |
| Configuration | Frontend contains hardcoded host URLs |
| Validation | Limited visible request validation |
| Security | No visible authentication or authorization |
| Observability | Limited visible structured logging |
| Health checks | No Kubernetes readiness or liveness probes identified |
| Dependencies | Docker base images use an unpinned `node:alpine` tag |

These observations describe the inspected implementation. They are not automatically requirements for the rebuilt application.

## 9. Architecture Evolution Strategy

The experiment will use two distinct stages.

### Stage 1 — Baseline Reproduction

**Initial implementation objective**

Rebuild the existing application while preserving its essential architecture and behavior.

The initial implementation should retain:

- React frontend.
- Five backend services.
- Existing service responsibilities and boundaries.
- Event-driven communication through the Event Bus.
- Query Service read model.
- In-memory data storage.
- Existing core user-facing capabilities.

The objective is to establish a working and comparable baseline before introducing architectural improvements.

### Stage 2 — Architecture Evolution

**Future improvement objective**

Evaluate potential improvements based on identified limitations and learning objectives.

Potential areas include:

- Persistent databases for posts and comments.
- Durable event storage and more reliable event delivery.
- Retry and failure-handling strategies.
- Observability and structured logging.
- Health checks and service resilience.
- Configuration and security improvements.
- Better automated testing and deployment practices.

Each proposed improvement should be evaluated and documented through an explicit architecture decision.

No Stage 2 improvement should be introduced implicitly during Stage 1.

## 10. Open Questions

The following questions remain relevant to architecture and implementation planning:

- Which Node.js versions should the rebuilt application use?
- How are container images currently built and made available to Kubernetes?
- What infrastructure is required to run the Kubernetes environment?
- How should event delivery failures be handled?
- What happens when a service is unavailable while an event is being forwarded?
- Should event history survive an Event Bus restart?
- What should happen to pending comments if moderation fails?
- How should users receive feedback about rejected comments?
- What are the intended visibility rules for pending and rejected comments?

These questions should be reviewed alongside the product and requirements documents.

## 11. Validation Boundaries

This document is based on static inspection of the original source code and deployment manifests, combined with the architecture decisions established for the experiment.

The following activities have not yet been performed:

- Building the original application.
- Running the application locally.
- Executing integration tests.
- Validating Kubernetes deployment.
- Testing event delivery and failure scenarios.
- Verifying runtime configuration and infrastructure.

Unverified runtime behavior must not be treated as an established fact.

## 12. Next Steps

After reviewing this architecture baseline:

1. Validate the architecture against the original application where practical.
2. Review requirements and identify architecture-related ambiguities.
3. Define the initial target architecture for Stage 1.
4. Identify service boundaries and implementation dependencies.
5. Document significant architecture decisions using Architecture Decision Records (ADRs).
6. Create an incremental implementation plan.

**Guiding principle:** Reproduce the existing architecture first. Evaluate improvements deliberately, with explicit decisions and evidence.