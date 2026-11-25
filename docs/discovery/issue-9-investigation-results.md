# Issue #9 — Technology Stack Investigation Results

**Issue:** [EPIC-01 / Confirm technology stack](https://github.com/l3ernardo/agenic-blog/issues/9)  
**Investigation:** Static inspection of the legacy application and current project documentation  
**Legacy application:** `C:\Users\bernlt\Desktop\Projetos\blog` (reference only)  
**Outcome:** Findings recorded for human review; no legacy files or dependencies were changed.

## Summary

The inspected legacy application has a React frontend and five Node.js/Express backend services: Posts, Comments, Moderation, Query, and Event Bus. The manifests declare the dependencies and version ranges summarized below; corresponding lockfiles contain resolved package versions. The Dockerfiles do not pin a Node.js release, and repository evidence does not establish the npm version.

This document records investigation evidence. It does not itself constitute approval of runtime versions or a replacement for a concise, approved stack baseline.

## Frontend

Evidence: `client\package.json`, `client\package-lock.json`, `client\Dockerfile`, and `client\src\App.js`.

- Framework: React, declared as `^18.3.1`; lockfile resolves React to `18.3.1`.
- DOM renderer: `react-dom`, declared as `^18.3.1`; lockfile resolves it to `18.3.1`.
- Build, development, and test scripts: Create React App through `react-scripts` `5.0.1`; the lockfile resolves `5.0.1`.
- HTTP client: Axios, declared as `^1.7.5`; lockfile resolves `1.7.5`.
- Other declared dependencies: `@testing-library/jest-dom` `^5.17.0`, `@testing-library/react` `^13.4.0`, `@testing-library/user-event` `^13.5.0`, and `web-vitals` `^2.1.4`.
- The package manifest defines `start`, `build`, `test`, and `eject` scripts using `react-scripts`.

The versions above distinguish manifest declarations from lockfile resolutions. The testing-library and `web-vitals` entries above are manifest declarations; this investigation does not report their resolved versions.

## Backend services

Evidence: each service's `package.json`, `package-lock.json`, `Dockerfile`, and `index.js`.

All five services use Express, declared as `^4.19.2` and resolved to `4.19.2` in their lockfiles. They use Axios, declared as `^1.7.5` and resolved to `1.7.5`, and Nodemon, declared as `^3.1.4` and resolved to `3.1.4`.

| Service | Port observed in source | Declared service-specific dependencies |
|---|---:|---|
| Posts | 4000 | CORS `^2.8.5` |
| Comments | 4001 | CORS `^2.8.5` |
| Moderation | 4003 | No CORS declaration |
| Query | 4002 | CORS `^2.8.5` |
| Event Bus | 4005 | No CORS declaration |

The CORS lockfile resolution is `2.8.5` for Posts, Comments, and Query.

All five entry points import `body-parser`, and all five lockfiles contain `body-parser` resolved to `1.20.2`. However, none of the five service manifests declares `body-parser` directly. It should therefore be described as a package present through the dependency tree and used by source code, not as an explicitly declared direct dependency.

Each backend manifest has a `start` script that runs `nodemon index.js`.

## Runtime, package manager, and container evidence

- The six Dockerfiles (frontend and five services) use `FROM node:alpine`. This is an unpinned tag; neither an exact Node.js version nor Alpine release is specified.
- The Dockerfiles do not pin an npm version. The manifests also do not specify Node.js or npm versions.
- Each lockfile uses `lockfileVersion: 3`. This is evidence about the lockfile format, not a basis for asserting an exact npm version.
- Each Dockerfile copies `package.json` into the image and runs `npm install` before copying the rest of the application. The lockfile is not copied before that install step, so the committed lockfile resolutions do not establish which exact dependency versions that Docker build installs.
- No runtime or package-manager version was inferred from current defaults.

**Recorded version status:** Node.js version unknown/unpinned; npm version unknown/unpinned.

## Deployment configuration inspected

The Kubernetes manifests in `infra\k8s` define Deployments and Services for the frontend and backend components, plus an NGINX Ingress. The image references do not provide evidence of the image's exact Node.js or npm versions. These files support the identification of the deployed components, not a more specific runtime baseline.

## Prior discovery documentation and discrepancies

Before this investigation note was added, `docs\discovery` contained no existing discovery report or technology-stack record. The existing `docs\architecture\architecture-overview.md` documented the React client, Node.js/Express services, five-service architecture, and unpinned `node:alpine` image tag, but did not record dependency versions.

The source inspection confirms that broad description and adds version evidence from manifests and lockfiles. It also makes the following qualifications explicit:

1. Node.js and npm versions are not pinned or verifiable from the inspected repository files.
2. The package lockfiles record resolved versions, while manifests record declared ranges or exact versions; these are not interchangeable.
3. `body-parser` is imported by every service and present in every lockfile but is not declared directly in the service manifests.
4. The Dockerfiles do not copy lockfiles before running `npm install`, so lockfile resolutions should not be presented as guaranteed container-build versions.
5. Backend dependency declarations differ by service: CORS is declared by Posts, Comments, and Query, but not Moderation or Event Bus.

## Files and configurations inspected

### New project documentation

- `AGENTS.md`
- `docs\architecture\architecture-overview.md`
- `docs\product\product-context.md`
- `docs\ai-first-sdlc-learning-guide.md`
- The pre-existing contents of `docs\discovery` (no discovery report was present)

### Legacy application source/configuration (read-only)

- `client\package.json`
- `client\package-lock.json`
- `client\Dockerfile`
- `client\src\App.js`
- `posts\package.json`, `posts\package-lock.json`, `posts\Dockerfile`, `posts\index.js`
- `comments\package.json`, `comments\package-lock.json`, `comments\Dockerfile`, `comments\index.js`
- `moderation\package.json`, `moderation\package-lock.json`, `moderation\Dockerfile`, `moderation\index.js`
- `query\package.json`, `query\package-lock.json`, `query\Dockerfile`, `query\index.js`
- `event-bus\package.json`, `event-bus\package-lock.json`, `event-bus\Dockerfile`, `event-bus\index.js`
- `infra\k8s\client-depl.yaml`
- `infra\k8s\posts-depl.yaml`, `infra\k8s\posts-srv.yaml`
- `infra\k8s\comments-depl.yaml`
- `infra\k8s\moderation-depl.yaml`
- `infra\k8s\query-depl.yaml`
- `infra\k8s\event-bus-depl.yaml`
- `infra\k8s\ingress-srv.yaml`

## Validation and limitations

- Findings were checked against the inspected manifests, lockfiles, Dockerfiles, entry points, and Kubernetes manifests.
- Inspection was static; no legacy application build, dependency installation, runtime execution, or Kubernetes deployment was performed.
- No files in the legacy application were modified, and no dependencies or runtimes were installed, upgraded, or removed.
- Human review is still required to accept the documented baseline as suitable to guide project setup.