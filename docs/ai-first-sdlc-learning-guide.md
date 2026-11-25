# AI-First Agentic SDLC Learning Guide

## Purpose

This guide defines the learning roadmap for rebuilding a simple blog application using AI-first and agentic engineering practices.

The existing blog application lives in a separate repository and must remain untouched. It serves as a reference for understanding the existing system. All new project changes must happen in this workspace.

The objective is not simply to reproduce the original application or generate code with AI. It is to experiment with an AI-first software development lifecycle, learning how humans and AI agents collaborate across discovery, product definition, requirements, architecture, implementation, testing, and deployment.

The workflow is deliberately incremental: establish a clear human-owned baseline, provide AI with explicit context and bounded responsibilities, and verify important results before moving to the next stage.

---

## Step 1: Establish the project workspace

**Status: Complete.**

- Create a dedicated project directory, separate from the original application.
- Add directories for discovery, product definition, requirements, architecture, and implementation records.
- Create the initial `AGENTS.md` and `README.md` files without prematurely defining project instructions.
- Do not add application code, dependencies, framework configuration, or agent definitions during scaffolding.
- Verify the directory structure and confirm that the original repository was not modified.

**Deliverable:** An empty, organized workspace ready for discovery and experimentation.

## Step 1.1: Establish version control and the project baseline

**Status: In progress.**

Before introducing AI project instructions or generating product documentation, establish a clean version-controlled baseline.

- Create a private GitHub repository.
- Configure the local Git repository to point to the correct GitHub repository.
- Add a `.gitignore` appropriate for the project.
- Verify that only files belonging to this project are included in Git.
- Create an initial commit containing the project scaffolding.
- Use `main` as the initial branch.
- Introduce feature branches and pull requests when implementation begins.

**Guardrails at this stage:**

- Keep the legacy application repository separate.
- Never commit credentials, secrets, or environment-specific configuration.
- Review changes before committing.
- Keep the initial Git configuration simple.

Do not introduce Git hooks, Husky, CI/CD pipelines, or additional tooling yet. These will be introduced when the project has a defined implementation stack, build process, and validation commands.

**Deliverable:** A clean, version-controlled project baseline on GitHub.

---

## Step 2: Discover and understand the existing system

**Status: Technical discovery completed; product discovery still requires validation.**

- Inspect the original application as a reference without editing it.
- Identify its components, services, integrations, data flows, and dependencies.
- Document existing functionality and observable user journeys.
- Identify architectural constraints, technical limitations, assumptions, and open questions.
- Distinguish verified implementation details from inferred behavior.
- Identify possible users and product needs, but do not treat assumptions as validated business requirements.
- Record findings in `docs/discovery/`.

**Important constraint:** Initially, the rebuild should preserve the existing application's architectural direction and core implementation characteristics. Architectural improvements will be considered in later iterations.

**Exit check:** Someone unfamiliar with the original application can understand its structure, behavior, constraints, and unresolved questions.

**Deliverable:** An existing-system discovery report and a documented set of product hypotheses.

---

## Step 3: Establish project instructions and AI operating context

- Define the purpose and objectives of the Agentic Engineering Lab.
- Establish the boundaries between the legacy application and the new project.
- Define the expected AI collaboration model.
- Document project conventions, working principles, and change-management expectations.
- Establish how AI should inspect context, identify assumptions, propose changes, and report results.
- Define which decisions require human review and approval.
- Create the initial `AGENTS.md`.

Keep instructions concise and focused on rules that apply across the project.

Do not introduce specialized agents, skills, or complex automation yet.

**Exit check:** An AI assistant working in the repository can understand the project objectives, its boundaries, and how it is expected to collaborate with the human.

**Deliverable:** An initial project-level `AGENTS.md` and documented AI operating principles.

---

## Step 4: Define the product

- Establish a product vision based on discovery findings and explicit assumptions.
- Identify the intended users and the problems the application should address.
- Define product goals and expected outcomes.
- Describe the primary user journeys and prioritize the smallest useful release.
- Identify in-scope and out-of-scope capabilities.
- Define success measures and how each one could be evaluated.
- Record the product scope and rationale in `docs/product/`.
- Use AI to help draft, challenge, and refine the product definition.
- Require human review and approval before proceeding.

**Exit check:** The intended first release, its users, scope, and success criteria are sufficiently clear to guide requirements engineering.

**Deliverable:** An approved product scope and product vision.

---

## Step 5: Write and validate requirements

- Convert prioritized product needs into clear functional requirements.
- Add relevant non-functional requirements, including accessibility, performance, privacy, security, and maintainability.
- Give each requirement a stable identifier and a way to verify it.
- Include acceptance criteria, edge cases, and relevant error behavior.
- Trace each requirement to its product goal and planned verification.
- Record requirements in `docs/requirements/`.
- Use AI to identify ambiguity, contradictions, missing cases, and inconsistencies.
- Keep humans responsible for final decisions and approval.

**Exit check:** Each in-scope requirement is sufficiently unambiguous to design and has an observable acceptance check.

**Deliverable:** A reviewed requirements specification with traceability and acceptance criteria.

---

## Step 6: Design and validate the architecture

- Start from the existing application's architecture and the constraints established during discovery.
- Compare suitable design options against the requirements and project constraints.
- Preserve the initial architecture unless a change is explicitly justified and approved.
- Document system boundaries, components, integrations, data flows, and key trade-offs in `docs/architecture/`.
- Record significant decisions with their context, alternatives, and consequences.
- Include operational, security, reliability, and failure considerations.
- Ask AI to challenge architectural proposals and identify risks.
- Resolve feedback explicitly before approving the design.

Architectural improvements should be treated as deliberate engineering decisions rather than automatic AI-generated redesigns.

**Exit check:** The architecture supports the agreed requirements, and unresolved decisions are documented.

**Deliverable:** An approved architecture specification and relevant architecture decision records.

---

## Step 7: Prepare bounded agent work

- Identify tasks that are suitable for AI execution.
- Define specialized agent instructions only for specific, repeatable responsibilities.
- Introduce reusable skills or workflows when recurring tasks justify them.
- Give each task a clear goal, relevant context, affected files, constraints, expected output, and validation commands.
- Establish how agents report assumptions, changes, and validation results.
- Keep humans responsible for scope, design decisions, review, and merging.
- Avoid giving agents broad permissions to change unrelated files or perform unreviewed external actions.

**Exit check:** Another contributor can understand what an agent may change, what it must not change, and how its work will be evaluated.

**Deliverable:** A set of bounded agent workflows and task definitions.

---

## Step 8: Implement in small, traceable increments

- Break approved requirements into small tasks with acceptance checks.
- Implement one vertical slice at a time.
- Use feature branches and pull requests when appropriate.
- Ask agents to explain assumptions and report changed files and checks performed.
- Compare implementation behavior against approved requirements, not only the agent's summary.
- Record implementation decisions and requirement links in `docs/implementation/`.
- Review and validate each increment before proceeding to dependent work.

**Exit check:** Each implementation increment is reviewable, traceable to requirements, and passes its acceptance checks.

**Deliverable:** A progressively implemented application with traceable changes.

---

## Step 9: Test and review continuously

- Derive tests from acceptance criteria and include important edge cases.
- Introduce automated tests, static checks, linting, and formatting as the implementation stack becomes established.
- Review generated code for correctness, maintainability, accessibility, and security-sensitive behavior.
- Introduce CI pipelines and automated quality gates when repeatable validation commands are available.
- Treat passing tests as evidence for covered behavior, not proof that all behavior is correct.
- Fix failures or document an explicit, reviewed reason for any accepted limitation.

**Exit check:** Required checks are reproducible, results are recorded, and known gaps are visible.

**Deliverable:** A verifiable quality baseline and repeatable validation workflow.

---

## Step 10: Deploy deliberately

- Choose a deployment approach consistent with the architecture and product needs.
- Verify configuration, secrets handling, migrations, monitoring, and rollback plans before release.
- Deploy to a non-production environment first.
- Exercise key user journeys and validate operational behavior.
- Require human approval for production-impacting actions.
- Record release steps and operational guidance in the relevant project documentation.

**Exit check:** The release can be observed, supported, and rolled back safely.

**Deliverable:** A reproducible deployment process and operational documentation.

---

## Step 11: Learn and improve the lifecycle

- Compare observed product outcomes with the success measures defined during product discovery.
- Collect user feedback and operational findings.
- Evaluate the effectiveness of AI collaboration and agent workflows.
- Identify where AI accelerated work and where human intervention was necessary.
- Revisit assumptions, requirements, architecture, and agent instructions when evidence shows they need to change.
- Record what worked, what failed, and what to try next.
- Introduce additional automation or guardrails based on demonstrated needs.

**Exit check:** Each iteration produces evidence that informs the next product and engineering improvements.

**Deliverable:** Documented learning outcomes and an improved AI-first engineering workflow.

---

## Working principles

- **Human accountability:** AI can propose and execute bounded tasks; people remain accountable for decisions, approvals, and releases.
- **Context before execution:** Provide AI with relevant project context, constraints, and objectives before assigning work.
- **Traceability:** Connect product goals to requirements, implementation, and verification.
- **Small batches:** Prefer changes that are easy to inspect, test, and revert.
- **Evidence over confidence:** Verify outputs with tests, review, and observed behavior.
- **Least privilege:** Give agents only the context and access required for their tasks.
- **Explicit uncertainty:** Record assumptions and unresolved questions rather than presenting guesses as facts.
- **Progressive automation:** Introduce agents, skills, hooks, and pipelines when they solve an identified problem.
- **Continuous learning:** Evaluate not only the software produced but also the effectiveness of the engineering process.

---

## Worked example: EPIC-01 Story — Confirm technology stack

This example records how GitHub issue [#9 — Confirm technology stack](https://github.com/l3ernardo/agenic-blog/issues/9) was taken from investigation through human review and completion. The issue belongs to [EPIC-01 — Project Foundation](https://github.com/l3ernardo/agenic-blog/issues/1).

### Objective and boundaries

The story's objective was to document the **existing blog application's** technology baseline so it can guide the initial rebuild. It was not a decision about AI-agent tools or the technology used to build agentic workflows.

The issue bounded the work to read-only inspection of the legacy manifests, lockfiles, Dockerfiles, entry points, and Kubernetes configuration. It required evidence-backed findings, explicit unknowns, no dependency or runtime changes, and human review before completion.

### Steps and artifacts

1. **Select and understand the tracked story.** Read the issue scope and acceptance criteria, its parent Epic, project instructions, and relevant discovery and architecture documentation.
2. **Inspect evidence without changing the legacy application.** Compare package manifests, lockfiles, Dockerfiles, source entry points, and deployment manifests. Keep declared dependency ranges distinct from lockfile resolutions; mark runtime or package-manager versions unknown where the evidence did not establish them.
3. **Record detailed findings.** [issue-9-investigation-results.md](./discovery/issue-9-investigation-results.md) captures the evidence inspected, findings, discrepancies, and limitations.
4. **Create the usable baseline record.** [technology-stack.md](./discovery/technology-stack.md) organizes the verified stack information for later project setup and links to the investigation report.
5. **Clarify scope with the human.** The review conversation distinguished the application's technology stack from agentic-development tooling.
6. **Review and close the issue.** The human moved the issue through review to Done and checked its acceptance criteria. GitHub now records it as closed/completed with status Done.

### Validation and outcome

This was a discovery and documentation story, so validation was static: findings were checked against the inspected source and configuration. No application build, runtime, dependency installation, or Kubernetes deployment was needed or performed. No legacy application files or dependencies were changed.

The story is complete as a **documented legacy baseline**. It does not choose currently unknown Node.js/npm versions or approve future implementation details beyond confirming that the documented evidence is suitable to guide project setup.

### Guardrail learned

Use a **backlog completion consistency check** before and after marking a story Done:

- Confirm each acceptance criterion is satisfied and checked, or explain an exception.
- Record the changed artifact(s), evidence, checks performed, and known limitations.
- Confirm the issue's workflow status and open/closed state match the completion outcome.
- Recheck the issue after the status update.

During this story, a status review found that the issue was Done while its acceptance checkboxes were still unchecked. The human corrected them, and a follow-up check confirmed consistency. This is a useful **human process guardrail**, not a technical enforcement mechanism. A short review comment recording the approval would make the rationale easier to audit later.

### What this example does not complete

This story demonstrates issue-based, bounded AI-assisted discovery and human review; it does not mean the overall agentic SDLC is complete. Product and requirements approval, the remaining foundation work, application implementation, automated testing, and deployment are separate future work. Specialized agents, CI, and technical enforcement were not required for this documentation story.

## Follow-up example: EPIC-01 Story — Establish application project structure

The next story, [#10 — Establish application project structure](https://github.com/l3ernardo/agenic-blog/issues/10), required a human-approved directory proposal before creating any structure.

- GitHub was marked Done before the application structure existed. The issue's checked acceptance boxes did not match the local worktree, which still had no application directories. The human clarified that only the issue status had been updated.
- The directory proposal was then presented for review. The human explicitly approved the root-level `client/`, `posts/`, `comments/`, `event-bus/`, `moderation/`, and `query/` structure and authorized minimal scaffolding.
- The approved directories, source placeholders, and minimal per-app `package.json` files were created. No dependencies, scripts, runtime versions, business logic, or service integrations were added.
- The approved structure and rationale were recorded in [project-structure.md](./implementation/project-structure.md). The six manifests were parsed as JSON and the expected source/public directories were checked.

**Learning:** A GitHub status and checked boxes are workflow metadata, not proof by themselves. Inspect the artifact and validation evidence before accepting completion. Since the GitHub integration used here is read-only, the human must add the completion evidence and document link to issue #10.