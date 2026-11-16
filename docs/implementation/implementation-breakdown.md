# Implementation Breakdown

**Status:** Initial Draft  
**Stage:** Task Decomposition & Agent Orchestration  
**Source:** `docs/implementation/implementation-plan.md`  
**Architecture:** `docs/architecture/target-architecture.md`

## 1. Purpose

This document translates the approved implementation plan into a structured hierarchy of engineering work.

The hierarchy will eventually be transferred into Jira when access becomes available.

The breakdown follows three levels:

- **Epic:** A significant area of implementation work.
- **Story:** A meaningful capability or outcome within an Epic.
- **Task:** A bounded engineering activity required to deliver a Story.

Detailed agent execution instructions will be prepared separately when we reach Agent Preparation.

## 2. Implementation Epics

### EPIC-01 — Project Foundation

**Objective:** Establish the technical foundation for the application.

Expected scope:
- Project structure.
- Technology stack confirmation.
- Dependency and package management.
- Environment configuration.
- Local development setup.

### EPIC-02 — Posts Service

**Objective:** Implement post creation and retrieval.

Expected scope:
- In-memory post storage.
- Post creation.
- Post retrieval.
- `PostCreated` event publication.

Related requirements: FR-001, FR-002.

### EPIC-03 — Comments Service

**Objective:** Implement comment creation, retrieval and moderation-state updates.

Expected scope:
- In-memory comment storage.
- Comment creation.
- Comment retrieval.
- Pending moderation state.
- Processing moderation outcomes.
- `CommentCreated` and `CommentUpdated` event publication.

Related requirements: FR-003, FR-004, FR-007, FR-008.

### EPIC-04 — Event Bus

**Objective:** Implement event reception, history and distribution.

Expected scope:
- Event reception.
- In-memory event history.
- Event distribution to subscribers.
- Event history retrieval.

Related requirements: Supports event-driven behavior across the application.

### EPIC-05 — Moderation Service

**Objective:** Implement automatic comment moderation.

Expected scope:
- Receive `CommentCreated`.
- Evaluate comment content.
- Produce approved or rejected outcomes.
- Publish `CommentModerated`.

Related requirements: FR-005, FR-006.

### EPIC-06 — Query Service

**Objective:** Implement the combined read model.

Expected scope:
- Process post and comment events.
- Maintain the combined projection.
- Retrieve event history during startup.
- Reconstruct the projection through event replay.
- Expose query endpoints.

Related requirements: FR-002, FR-004, FR-009, FR-010.

### EPIC-07 — React Client

**Objective:** Implement the user-facing blog experience.

Expected scope:
- Display posts.
- Display comments.
- Create posts.
- Submit comments.
- Display moderation states and feedback.

Related requirements: US-001 through US-007.

### EPIC-08 — Integration & Validation

**Objective:** Validate the complete application against the agreed baseline.

Expected scope:
- Service integration.
- Event-flow validation.
- End-to-end behavior.
- Acceptance criteria verification.
- Architecture consistency review.

## 3. Dependencies and Implementation Considerations

The initial implementation sequence follows the implementation plan, but dependencies must be considered:

- Project Foundation precedes service implementation.
- Posts and Comments can begin with their core HTTP and in-memory behavior.
- Event Bus integration is required for event publication and consumption.
- Moderation depends on CommentCreated event delivery.
- Comments must process moderation outcomes.
- Query Service depends on the relevant events.
- React Client depends on the backend interfaces it consumes.
- Integration & Validation requires the application components to be available.

The exact order of parallel work should be refined during task decomposition.

## 4. Breakdown Status

| Epic | Breakdown status |
| EPIC-01 — Project Foundation | Proposed |
| EPIC-02 — Posts Service | Proposed |
| EPIC-03 — Comments Service | Proposed |
| EPIC-04 — Event Bus | Proposed |
| EPIC-05 — Moderation Service | Proposed |
| EPIC-06 — Query Service | Proposed |
| EPIC-07 — React Client | Proposed |
| EPIC-08 — Integration & Validation | Proposed |

Stories and detailed engineering tasks will be added progressively after reviewing each Epic.

## 5. Jira Migration

When Jira becomes available:

- Create one Epic for each implementation area.
- Create Stories under the relevant Epics.
- Create engineering Tasks under Stories where appropriate.
- Transfer acceptance criteria and dependencies.
- Preserve references to requirements and architecture documentation.
- Use Jira as the source of truth for work status and ownership.

## 6. Important Boundaries

This breakdown follows Stage 1 — Baseline Reproduction.

It does not authorize:
- Introducing persistent databases.
- Replacing the Event Bus with a different messaging platform.
- Consolidating microservices.
- Adding new product capabilities.
- Introducing unrelated architectural improvements.

Any proposed deviation requires human review and, where appropriate, an Architecture Decision Record.