# Application Project Structure

**Status:** Approved and scaffolded  
**Story:** [EPIC-01 / Establish application project structure](https://github.com/l3ernardo/agenic-blog/issues/10)  
**Architecture reference:** `docs/architecture/target-architecture.md`

## Purpose

Establish independent application directories for the React client and the five backend services while preserving the approved Stage 1 architecture.

## Approved structure

```text
client/
  package.json
  public/
  src/
posts/
  package.json
  src/
comments/
  package.json
  src/
event-bus/
  package.json
  src/
moderation/
  package.json
  src/
query/
  package.json
  src/
```

Each application has its own package manifest. The source and public directories contain placeholders only; implementation belongs to later stories.

## Rationale and boundaries

- Root-level directories retain the legacy application's clear service boundaries.
- Each application can receive its own dependencies and execution configuration without introducing a root workspace or shared package structure.
- The React client's `public/` directory is separate from its source directory.
- No dependencies, scripts, runtime versions, business logic, service integrations, or deployment configuration were added.
- The manifests are minimal placeholders; local run and validation commands remain part of subsequent Project Foundation work.

## Validation

The resulting tree and package manifests were inspected after scaffolding. This story establishes structure only; it does not claim that any application can run yet.
