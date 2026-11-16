# Implementation Plan — Stage 1 Baseline Reproduction

**Status:** Initial Draft  
**Implementation stage:** Stage 1 — Baseline Reproduction  
**Architecture reference:** `docs/architecture/target-architecture.md`  
**Decision reference:** `docs/architecture/architecture-decision-records.md`

## 1. Purpose

This document defines the initial implementation strategy for rebuilding the existing blog application.

The objective is to translate the approved target architecture and requirements into small, manageable implementation increments.

The implementation will follow an incremental approach, allowing each component and integration to be reviewed and validated before moving forward.

## 2. Implementation Principles

The implementation will follow these principles:

- Preserve the existing architecture and essential behavior.
- Implement one bounded capability at a time.
- Maintain clear service ownership and responsibilities.
- Keep implementation tasks traceable to requirements.
- Validate components before integrating them.
- Use AI assistants and agents within explicitly defined task boundaries.
- Require human review of significant changes.
- Avoid unrelated refactoring and unnecessary dependencies.

## 3. Implementation Strategy

Implementation will be organized into sequential milestones.

### Milestone 1 — Project Foundation

**Objective:** Establish the technical foundation for implementation.

Activities:

- Confirm the application technology stack using the legacy source.
- Establish the frontend and backend project structure.
- Define package and dependency management.
- Establish environment configuration conventions.
- Prepare local development and execution commands.
- Establish basic code quality and validation practices.

Expected outcome:

A clean project foundation that supports the implementation of individual services.

### Milestone 2 — Core Business Services

**Objective:** Implement the two primary business services.

#### Posts Service

Activities:

- Implement post creation.
- Implement post retrieval.
- Maintain post data in memory.
- Publish `PostCreated`.
- Validate the relevant functional requirements.

#### Comments Service

Activities:

- Implement comment creation.
- Associate comments with posts.
- Maintain comments in memory.
- Initially assign pending moderation status.
- Process `CommentModerated`.
- Publish `CommentCreated` and `CommentUpdated`.
- Validate the relevant functional requirements.

Expected outcome:

Both core business services operate independently and expose their expected HTTP interfaces.

### Milestone 3 — Event Bus

**Objective:** Implement event distribution between services.

Activities:

- Implement event reception.
- Maintain in-memory event history.
- Implement event forwarding to subscribers.
- Expose event history retrieval.
- Validate communication with service consumers.

Expected outcome:

Services can publish and receive events through the Event Bus.

### Milestone 4 — Moderation Service

**Objective:** Implement comment moderation.

Activities:

- Subscribe to `CommentCreated`.
- Evaluate submitted comments.
- Reproduce the existing moderation rule.
- Publish `CommentModerated`.
- Validate the interaction with the Comments Service.

Expected outcome:

Submitted comments receive moderation outcomes through event-driven communication.

### Milestone 5 — Query Service

**Objective:** Implement the combined read model.

Activities:

- Process `PostCreated`.
- Process `CommentCreated`.
- Process `CommentUpdated`.
- Maintain an in-memory read projection.
- Retrieve event history during startup.
- Reconstruct the read model through event replay.
- Expose the expected query interface.

Expected outcome:

The Query Service provides a combined view of posts and comments.

### Milestone 6 — React Client

**Objective:** Implement the user-facing application.

Activities:

- Display available posts.
- Display associated comments.
- Implement post creation.
- Implement comment submission.
- Retrieve data through the Query Service.
- Represent relevant moderation states and feedback.
- Validate the user-facing acceptance criteria.

Expected outcome:

Users can interact with the application through the React frontend.

### Milestone 7 — System Integration and Validation

**Objective:** Validate the complete application.

Activities:

- Start all required services.
- Validate service-to-service communication.
- Validate post creation and retrieval.
- Validate comment submission and moderation.
- Validate event propagation.
- Validate Query Service projections.
- Validate frontend behavior.
- Review implementation against requirements and architecture documentation.

Expected outcome:

A working baseline implementation that reproduces the essential behavior of the legacy application.

## 4. Traceability

Implementation tasks should maintain a relationship with approved requirements.

The following mapping is an initial guide:

| Component | Related requirements |
|---|---|
| Posts Service | FR-001, FR-002 |
| Comments Service | FR-003, FR-004, FR-007, FR-008 |
| Moderation Service | FR-005, FR-006 |
| Query Service | FR-002, FR-004, FR-009 |
| React Client | US-001 through US-007 |
| Event Bus | Supporting integration for FR-001 through FR-009 |

The mapping should be refined as implementation details and acceptance criteria are validated.

## 5. Validation Strategy

Each milestone should include validation before moving to the next major integration.

Validation may include:

- Unit tests for service behavior.
- API-level tests for HTTP endpoints.
- Integration tests for event communication.
- Read-model consistency checks.
- Frontend behavior validation.
- Acceptance criteria verification.
- Human code review.

The exact testing tools and framework will be selected after the technology stack is confirmed.

## 6. AI Collaboration Strategy

AI participation should evolve alongside implementation.

For each task, the AI assistant should receive:

- Relevant project instructions.
- Target architecture context.
- Related requirements and acceptance criteria.
- Relevant source files and dependencies.
- Explicit task boundaries.
- Expected validation criteria.

AI should work on small, bounded tasks rather than independently implementing entire system areas without review.

The human remains responsible for:

- Approving implementation scope.
- Reviewing architectural consistency.
- Evaluating generated code.
- Validating behavior.
- Approving significant changes.

Specialized agents may be introduced later when their responsibilities and boundaries are clear.

## 7. Out of Scope for Stage 1

The following improvements remain deferred:

- Persistent databases.
- Durable messaging infrastructure.
- Advanced event delivery guarantees.
- Advanced observability platforms.
- New authentication and authorization capabilities.
- Additional product functionality.
- Architectural redesign.

These topics belong to Stage 2 unless an explicit decision identifies a necessary change to the baseline.

## 8. Completion Criteria

Stage 1 implementation can be considered complete when:

- All five backend services are implemented.
- The React Client supports the existing core user-facing capabilities.
- Event-driven communication works across the relevant services.
- The Query Service maintains the expected read model.
- The application passes the agreed functional acceptance criteria.
- Integration behavior has been validated.
- Architecture deviations are documented.
- Remaining limitations and unverified behavior are explicitly recorded.

## 9. Next Steps

Before implementation begins:

1. Review and approve the target architecture.
2. Review and approve the initial architecture decisions.
3. Review the implementation sequence.
4. Confirm the technology stack and local execution approach.
5. Break the first milestone into bounded implementation tasks.

**Guiding principle:** Implement incrementally, validate continuously, and keep every AI-assisted change traceable to an agreed requirement or architecture decision.