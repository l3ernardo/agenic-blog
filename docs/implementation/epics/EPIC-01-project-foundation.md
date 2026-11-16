# EPIC-01 — Project Foundation

**Status:** Proposed  
**Phase:** Task Decomposition & Agent Orchestration  
**Implementation milestone:** Project Foundation  
**Related documents:**
- `docs/implementation/implementation-plan.md`
- `docs/implementation/implementation-breakdown.md`
- `AGENTS.md`
- `docs/architecture/target-architecture.md`

## 1. Epic Overview

### Objective

Establish the technical foundation required to implement the Agentic Blog application.

This Epic prepares the development environment, application structure, project configuration and basic validation approach.

The goal is to create a consistent foundation that allows subsequent Epics to implement the individual services and frontend.

### Expected outcome

At the end of this Epic:

- The technology stack and runtime assumptions are documented.
- The application directory structure is established.
- Frontend and backend project configurations are in place.
- Local development configuration and service ports are documented.
- Basic development and validation commands are available.
- A developer can understand how to prepare and run the project.

### Scope

This Epic focuses on project setup and development foundations.

It does not implement business functionality or introduce architectural improvements.

The initial implementation should remain aligned with the existing application baseline.

---

## 2. Dependencies and Inputs

Before starting this Epic, the following information should be available:

| Input | Source |
|---|---|
| Existing frontend technology and dependencies | Discovery report and legacy source code |
| Existing backend technology and dependencies | Discovery report and legacy source code |
| Existing service responsibilities | Architecture overview |
| Target architecture | `target-architecture.md` |
| Project-wide engineering instructions | `AGENTS.md` |

### Important considerations

Some details remain unverified and must be inspected before decisions are made:

- Exact Node.js runtime version.
- Package manager and version.
- Existing dependency versions and compatibility.
- Whether the new project should reproduce the original directory organization or use a slightly different organization while preserving the same architecture.

Do not assume these details. Inspect the original application and document the findings.

---

## 3. Stories and Tasks

### STORY-01 — Confirm the Technology Stack

**Objective:** Identify and document the technologies, frameworks, dependencies and runtime assumptions required to reproduce the existing application.

**Tasks:**

- TASK-01.1 — Inspect the legacy frontend dependencies and configuration.
- TASK-01.2 — Inspect the dependencies and configuration of each backend service.
- TASK-01.3 — Identify the Node.js runtime and package manager requirements.
- TASK-01.4 — Document the confirmed technology stack and unresolved questions.

**Expected outcome:** A documented technology baseline that can guide project initialization.

### STORY-02 — Establish the Application Project Structure

**Objective:** Define and create the directory and package structure for the new application.

**Tasks:**

- TASK-02.1 — Review the existing application directory organization.
- TASK-02.2 — Propose the new application directory structure.
- TASK-02.3 — Validate the proposed structure against the target architecture.
- TASK-02.4 — Initialize the frontend project configuration.
- TASK-02.5 — Initialize the backend service project configurations.

**Expected outcome:** A consistent project structure with the necessary frontend and backend project configurations.

**Constraint:** The structure must support the existing service boundaries. Do not consolidate services or introduce new architectural patterns without an explicit decision.

### STORY-03 — Configure the Local Development Environment

**Objective:** Define the configuration required to develop and run the application locally.

**Tasks:**

- TASK-03.1 — Define the local service port allocation.
- TASK-03.2 — Define environment variable and configuration conventions.
- TASK-03.3 — Establish local service communication configuration.
- TASK-03.4 — Define basic development start and stop commands.
- TASK-03.5 — Document the local development setup instructions.

**Expected outcome:** Developers can understand how the application services are configured and how to start the development environment.

### STORY-04 — Establish Development and Validation Foundations

**Objective:** Establish the minimum conventions and validation approach needed before implementing business functionality.

**Tasks:**

- TASK-04.1 — Define basic code organization and naming conventions.
- TASK-04.2 — Identify the initial testing approach and tools.
- TASK-04.3 — Establish basic build and validation commands.
- TASK-04.4 — Document how to execute the available validation checks.

**Expected outcome:** The project has a documented and repeatable baseline for development and validation.

---

## 4. Dependencies Between Stories

The recommended execution sequence is:

1. STORY-01 — Confirm the Technology Stack.
2. STORY-02 — Establish the Application Project Structure.
3. STORY-03 — Configure the Local Development Environment.
4. STORY-04 — Establish Development and Validation Foundations.

STORY-03 and STORY-04 can partially overlap once the project structure is established.

The technology stack must be confirmed before initializing project configurations.

---

## 5. Epic Acceptance Criteria

EPIC-01 can be considered complete when:

- [ ] The technology stack and runtime assumptions are documented.
- [ ] Unverified technology assumptions are identified.
- [ ] The application directory structure is established.
- [ ] Frontend and backend project configurations are initialized.
- [ ] Local service ports and configuration conventions are documented.
- [ ] Basic development commands are available.
- [ ] Initial build and validation commands are documented and executable where applicable.
- [ ] Setup instructions allow another developer to understand how to prepare the project.

---

## 6. AI Contribution

AI can assist with:

- Inspecting the existing source code and configuration files.
- Comparing legacy and proposed project structures.
- Identifying dependency and runtime requirements.
- Proposing configuration and setup files.
- Generating initial scripts and documentation.
- Executing available build and validation commands.
- Identifying inconsistencies between the implementation and architecture documentation.

AI-generated configurations and recommendations must be reviewed before adoption.

## 7. Human Responsibility

The human engineering owner remains responsible for:

- Validating the technology baseline.
- Approving the project structure.
- Approving dependency and runtime decisions.
- Ensuring the new project remains aligned with the agreed architecture.
- Reviewing generated configuration and scripts.
- Confirming that the development environment works as expected.

AI assists with analysis and implementation, but does not independently approve architectural or technical decisions.

## 8. Out of Scope

The following activities are intentionally excluded:

- Implementing business functionality.
- Implementing individual microservices.
- Introducing a database or persistent storage.
- Introducing a durable message broker.
- Adding authentication or authorization.
- Implementing CI/CD pipelines.
- Introducing advanced agent orchestration or automation.
- Refactoring the original architecture.

These activities belong to later Epics or require separate architectural decisions.

---

## 9. Learning Objectives

This Epic provides an opportunity to learn how AI can support project initialization without taking ownership of engineering decisions.

Key learning points:

- AI can inspect an existing codebase and extract technical requirements.
- AI can propose project structures and configuration.
- Human review is necessary before adopting generated configurations.
- Project instructions provide context, but do not replace technical validation.
- A repeatable development environment is part of the engineering foundation, not merely a setup activity.

**Guiding principle:** AI can accelerate project preparation, but humans remain accountable for the technical foundation.