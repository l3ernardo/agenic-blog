# AGENTS.md

## 1. Project Overview

This project is the Agentic Engineering Lab, a practical learning environment for studying and applying AI-first software development and agentic engineering principles.

The initial experiment is to rebuild an existing event-driven blog application using an incremental, AI-first Software Development Lifecycle (SDLC).

The objective is to learn how humans and AI agents collaborate across product discovery, requirements, architecture, implementation, testing, and deployment.

## 2. Experiment Objectives

- Apply AI-first engineering practices to a real software project.
- Explore how AI can participate in different SDLC activities.
- Learn how to provide effective context and bounded responsibilities to AI assistants and agents.
- Evaluate AI-generated decisions and outputs through human review and technical validation.
- Introduce agents, skills, and automation progressively as the project evolves.
- Document lessons learned and continuously improve the engineering workflow.

## 3. Project Boundaries

- The original blog application exists in a separate repository and is strictly read-only.
- Use the original application only as a reference for understanding existing functionality, architecture, and implementation.
- All new development and documentation must happen in this repository.
- Do not modify, delete, or overwrite files in the original application.
- Initially preserve the original application's architectural direction and core implementation characteristics.
- Do not introduce architectural improvements or additional product capabilities without an explicit decision and documented rationale.

## 4. Engineering Principles

- **Context before execution:** Inspect relevant project documentation before starting a task.
- **Human accountability:** Humans own product decisions, requirements approval, architecture decisions, and releases.
- **Incremental delivery:** Prefer small, focused, reviewable changes.
- **Traceability:** Maintain relationships between product goals, requirements, implementation, and verification.
- **Evidence over confidence:** Verify outputs through review, tests, and observable behavior.
- **Explicit uncertainty:** Distinguish verified facts, assumptions, recommendations, and open questions.
- **Progressive automation:** Introduce additional tooling and automation only when justified by project needs.

## 5. AI Collaboration Guidelines

Before executing a task:

- Understand the objective and inspect relevant existing documentation.
- Identify constraints, dependencies, and affected files.
- Clarify ambiguity rather than inventing requirements.
- Explain significant assumptions and proposed decisions.

When executing a task:

- Stay within the requested scope.
- Avoid unnecessary modifications or unrelated refactoring.
- Prefer existing project conventions and documented decisions.
- Do not introduce dependencies, frameworks, agents, or tools without justification and approval.

After executing a task:

- Summarize what was accomplished.
- Identify files created or modified.
- Report validation performed and any checks not performed.
- Highlight remaining questions, risks, or limitations.

## 6. Change Management and Guardrails

- Review existing changes before modifying files.
- Never overwrite existing work without authorization.
- Do not modify the legacy application repository.
- Do not commit credentials, secrets, or sensitive environment configuration.
- Require human review and approval

##
Correct. AGENTS.md is project-level guidance for AI coding assistants, not a security enforcement mechanism.

It serves two purposes:
1. Context: Explains what the project is, its objectives, architecture constraints, and engineering principles.
2. Behavioral guidance: Tells AI assistants how they should work in this repository, including boundaries, review expectations, and what requires human approval.

As we progress, we can distinguish three layers:
- Instructions: What agents should do (AGENTS.md).
- Guardrails: What agents are technically allowed to do.
- Validation: How we verify their work (tests, reviews, CI).

##

## Diference between Guidance x Enforcement
Guidance only = a rule the agent is expected to follow.



Enforcement = a restriction the agent cannot bypass through its normal permissions and tools.

One way to remember it as you continue reading:
- Context gives the agent understanding.
- Instructions guide its behavior.
- Guardrails constrain its actions.
- Validation provides evidence that its work is correct.
- Human oversight maintains accountability.