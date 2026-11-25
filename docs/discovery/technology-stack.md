# Verified Technology Stack Baseline

**Status:** Human-reviewed and accepted as the legacy application baseline  
**Scope:** Existing application baseline, based on the Issue #9 investigation report  
**Evidence:** [Issue #9 investigation results](./issue-9-investigation-results.md)

## 1. Purpose and evidence conventions

This document records the technology baseline that the new implementation should initially reproduce. It reports declared dependency ranges separately from lockfile-resolved versions. A resolved lockfile version is not a guarantee of the version installed by the inspected Docker build, because those Dockerfiles do not copy the lockfile before running `npm install`.

The findings below are limited to what the investigation report establishes. Unknown or unpinned values are identified explicitly rather than inferred.

## 2. Frontend

The frontend is a React application using Create React App tooling.

| Technology or dependency | Manifest declaration | Lockfile resolution | Notes |
|---|---|---|---|
| React | `^18.3.1` | `18.3.1` | UI framework |
| React DOM | `^18.3.1` | `18.3.1` | DOM renderer |
| `react-scripts` | `5.0.1` | `5.0.1` | Create React App start, build, test, and eject scripts |
| Axios | `^1.7.5` | `1.7.5` | HTTP client |
| `@testing-library/jest-dom` | `^5.17.0` | Not recorded in the investigation report | Declared dependency |
| `@testing-library/react` | `^13.4.0` | Not recorded in the investigation report | Declared dependency |
| `@testing-library/user-event` | `^13.5.0` | Not recorded in the investigation report | Declared dependency |
| `web-vitals` | `^2.1.4` | Not recorded in the investigation report | Declared dependency |

The manifest scripts are `start`, `build`, `test`, and `eject`, all using `react-scripts`.

## 3. Backend

The backend consists of five Node.js services using Express: Posts, Comments, Moderation, Query, and Event Bus. The investigation report identifies these common manifest declarations and lockfile resolutions:

| Dependency | Manifest declaration in all five services | Lockfile resolution |
|---|---|---|
| Express | `^4.19.2` | `4.19.2` |
| Axios | `^1.7.5` | `1.7.5` |
| Nodemon | `^3.1.4` | `3.1.4` |

Each service manifest defines a `start` script that runs `nodemon index.js`.

### Service-specific differences

| Service | Port observed in source | Additional declared dependency |
|---|---:|---|
| Posts | 4000 | CORS `^2.8.5`, resolved to `2.8.5` |
| Comments | 4001 | CORS `^2.8.5`, resolved to `2.8.5` |
| Moderation | 4003 | None reported |
| Query | 4002 | CORS `^2.8.5`, resolved to `2.8.5` |
| Event Bus | 4005 | None reported |

The report establishes that CORS is declared by Posts, Comments, and Query, but not by Moderation or Event Bus. It does not establish whether those latter services use CORS through another mechanism.

## 4. Direct and transitive dependencies

- The frontend dependencies listed in its manifest are direct declarations. The report provides resolved versions for React, React DOM, `react-scripts`, and Axios; it does not provide lockfile resolutions for the testing-library packages or `web-vitals`.
- Express, Axios, and Nodemon are directly declared by each backend service manifest.
- CORS is directly declared by Posts, Comments, and Query only.
- All five backend entry points import `body-parser`, and all five service lockfiles contain it resolved to `1.20.2`. None of the five service manifests declares it directly. The investigation report identifies it as present transitively through Express.
- The backend `body-parser` imports therefore rely on a transitive package rather than a direct manifest declaration. This is a technical observation, not a change made as part of this baseline record.

## 5. Runtime and package manager

| Item | Verified status |
|---|---|
| Node.js version | Unknown and unpinned. Dockerfiles use the mutable `node:alpine` tag; no exact Node.js or Alpine version is recorded. |
| npm version | Unknown and unpinned. The Dockerfiles invoke `npm`, but do not select a version. |
| Lockfile format | All six package lockfiles use `lockfileVersion: 3`. This does not establish the exact npm version. |

No specific Node.js, Alpine, or npm version should be inferred from these findings.

## 6. Docker build behavior and lockfile limitation

The frontend and each backend service have a Dockerfile that uses `FROM node:alpine`. The reported build sequence copies `package.json`, runs `npm install`, then copies the application files. The lockfile is not copied before installation.

Consequently, the committed lockfile resolutions document the repository's recorded dependency tree, but do not establish which exact dependency versions are installed during this Docker build. The Node.js and Alpine versions are also not fixed by the tag.

## 7. Kubernetes deployment

The inspected Kubernetes configuration uses Deployments and Services for the frontend and backend components, plus an NGINX Ingress. This establishes Kubernetes and NGINX Ingress as observed deployment technologies.

The investigation report does not establish Kubernetes cluster/version requirements, the NGINX Ingress Controller version, exact runtime versions inside deployed images, or how images are built and published. Deployment image references do not provide evidence for those values.

## 8. Discrepancies and technical observations

1. The existing architecture overview already identified React, Node.js/Express, the five backend services, and the unpinned `node:alpine` image tag, but did not record dependency versions.
2. The investigation report distinguishes manifest declarations from lockfile resolutions. This document preserves that distinction; range declarations such as `^4.19.2` are not restated as exact installed versions.
3. `body-parser` is imported by every backend service and resolved in each lockfile, but is absent from each service's direct dependencies.
4. Dockerfiles run `npm install` without first copying lockfiles, so lockfile-resolved versions cannot be asserted as the versions installed by those image builds.
5. Backend CORS declarations are not uniform: present in Posts, Comments, and Query manifests; absent in Moderation and Event Bus manifests.
6. Node.js and npm versions are not pinned or verifiable from the inspected evidence.

## 9. Remaining unknowns and assumptions

- Exact Node.js and Alpine release versions.
- Exact npm version used to create the lockfiles or run the Docker builds.
- Dependency versions actually installed in the Docker images.
- Lockfile-resolved versions for the frontend testing-library packages and `web-vitals`.
- Kubernetes and NGINX Ingress Controller versions and cluster configuration.
- Image build and publishing process.
- Whether the undeclared `body-parser` dependency is intentionally relied on as a transitive dependency.
- Whether services without a CORS manifest declaration have any other CORS configuration.

This is a static, report-based technology record. Runtime behavior, builds, dependency installation, and deployment were not independently verified as part of this documentation task. GitHub issue [#9 — Confirm technology stack](https://github.com/l3ernardo/agenic-blog/issues/9) records the human review and is closed as Done. Acceptance of this baseline does not resolve the runtime and deployment unknowns listed above.
