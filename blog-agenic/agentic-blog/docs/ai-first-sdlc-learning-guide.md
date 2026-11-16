# AI-First Agentic SDLC Learning Guide

This guide is a learning roadmap for rebuilding a simple blog as an agentic
engineering experiment. The existing blog lives in a separate repository and
must remain untouched. Treat it as a reference only; make all project changes
in this workspace.

The workflow is deliberately incremental: establish a clear human-owned
baseline, give AI agents bounded work with explicit context, and verify every
important result before moving to the next stage.

## Step 1: Establish the project workspace

**Status: complete.**

- Create a dedicated project directory, separate from the original application.
- Add places for discovery, product definition, requirements, architecture,
  and implementation records.
- Keep the initial `AGENTS.md` and `README.md` empty until their purpose and
  project conventions are decided.
- Do not add application code, dependencies, framework configuration, or agent
  definitions during scaffolding.
- Verify the directory structure and confirm that the original repository was
  not changed.

**Deliverable:** an empty, organized workspace ready for discovery.

## Pre-Step before we start with the Discovery and define the product we need toc rate ao GitRepo on GitHub

Before creating AGENTS.md, I would establish this small checklist:
- [ ] Create the private GitHub repository.
- [ ] Push the initial scaffolding commit.
- [ ] Add a .gitignore appropriate for the project.
- [ ] Establish a basic branch strategy.
- [ ] Configure available repository protections.
- [ ] Then create AGENTS.md with the initial operating instructions.

** I would not install hooks, Husky, CI/CD or additional tooling yet.
Before pushing project structure create the .gitignore at its minimal.

## Step 2: Discover the problem and users

- Identify the intended users and the problem the blog solves for them.
- Inspect the original application as a reference without editing it.
- Record observed features, user journeys, constraints, assumptions, and open
  questions in `docs/discovery/`.
- Separate verified facts from guesses; confirm important assumptions with a
  stakeholder or label them for later validation.
- Agree on what is explicitly out of scope.

**Exit check:** the problem, users, constraints, and unresolved questions are
visible and understandable to someone who did not do the discovery.

## Step 3: Define the product

- Turn discovery findings into a concise product vision and measurable goals.
- Describe the primary user journeys and prioritize the smallest useful
  release.
- Define success measures and identify how each one could be evaluated.
- Record feature priorities and rationale in `docs/product/`.
- Review scope with a human before asking agents to design or implement it.

**Exit check:** the intended first release and its success criteria are agreed
and testable.

## Step 4: Write and validate requirements

- Convert prioritized product needs into clear functional requirements.
- Add non-functional requirements such as accessibility, performance,
  privacy, security, and maintainability where they apply.
- Give each requirement a stable identifier and a way to verify it.
- Include acceptance criteria, edge cases, and relevant error behavior.
- Trace each requirement to its product goal and planned verification in
  `docs/requirements/`.
- Use an AI agent to find ambiguity, contradictions, and missing cases; a human
  owns final decisions and approval.

**Exit check:** each in-scope requirement is unambiguous enough to design and
has an observable acceptance check.

## Step 5: Design the architecture

- Compare a small number of suitable designs against the requirements and
  project constraints.
- Document the chosen design, key boundaries, data flows, and important
  trade-offs in `docs/architecture/`.
- Record significant decisions with their context, alternatives, and
  consequences.
- Include operational, security, and failure considerations in the design.
- Ask an AI agent to challenge the proposal, then resolve feedback explicitly.

**Exit check:** the design supports the agreed requirements, and unresolved
architectural decisions are tracked rather than silently assumed.

## Step 6: Prepare bounded agent work

- Once project conventions are understood, write them down in `AGENTS.md`.
- Define agent instructions only for specific, repeatable roles or tasks.
- Give each task a narrow goal, relevant files and requirements, constraints,
  expected output, and validation commands.
- Keep humans responsible for scope, design choices, review, and merging.
- Avoid giving an agent broad permission to change unrelated files or make
  unreviewed external changes.

**Exit check:** another contributor can tell what an agent may change, what it
must not change, and how its result will be checked.

## Step 7: Implement in small, traceable increments

- Break approved requirements into small tasks with acceptance checks.
- Implement one vertical slice at a time, keeping changes reviewable.
- Ask agents to explain assumptions and report changed files and checks run.
- Compare implementation behavior against requirements, not only against the
  agent's summary.
- Record implementation notes and requirement links in `docs/implementation/`.

**Exit check:** each increment is reviewed and its acceptance checks pass
before dependent work proceeds.

## Step 8: Test and review continuously

- Derive tests from acceptance criteria and include important edge cases.
- Run the relevant automated tests, static checks, and build after changes.
- Review generated code for correctness, maintainability, accessibility, and
  security-sensitive behavior.
- Treat passing tests as evidence for covered behavior, not proof that all
  behavior is correct.
- Fix failures or document an explicit, reviewed reason for any accepted
  limitation.

**Exit check:** required checks are reproducible, results are recorded, and
known gaps are visible to the team.

## Step 9: Deploy deliberately

- Choose a deployment approach consistent with the architecture and product
  needs.
- Verify configuration, secrets handling, migrations, monitoring, and rollback
  plans before release.
- Deploy to a non-production environment first and exercise key user journeys.
- Require human approval for production-impacting actions.
- Record release steps and operational guidance in the relevant project docs.

**Exit check:** the release can be observed, supported, and rolled back safely.

## Step 10: Learn and improve the lifecycle

- Compare observed product outcomes with the success measures from discovery.
- Collect user feedback and operational findings.
- Revisit assumptions, requirements, architecture, and agent instructions when
  evidence shows they need to change.
- Keep a short record of what worked, what failed, and what to try next.
- Start the next iteration with the highest-value validated learning.

**Exit check:** each iteration produces evidence that informs the next one.

## Working principles

- **Human accountability:** AI can propose and execute bounded tasks; people
  remain accountable for decisions and releases.
- **Traceability:** connect product goals to requirements, implementation, and
  verification.
- **Small batches:** prefer changes that are easy to inspect, test, and revert.
- **Evidence over confidence:** verify outputs with tests, review, and observed
  behavior.
- **Least privilege:** provide agents only the context and access required for
  their task.
- **Explicit uncertainty:** record assumptions and unresolved questions rather
  than presenting guesses as facts.
