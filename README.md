# platform-standards

## Purpose
This repository is the single source of truth for engineering standards in `github.com/qabasone` for the `qabas.one` internal platform.

It defines enforceable standards for:
- human developers
- AI development agents
- automated engineering tooling
- future platform teams

## Scope
These standards apply to all repositories and services in the platform, including:
- `api-gateway`
- `api-auth`
- `api-identity`
- future services such as policy, workflow, logistics, finance, analytics, and AI capabilities

## Rules
1. Treat all standards in this repository as normative unless an explicit exception is approved.
2. Do not duplicate or redefine standards in service repositories; link back to this repository.
3. When a standard changes, update related templates in `templates/` in the same pull request.
4. If implementation conflicts with a standard, raise an ADR and update the standard before rollout.

## How To Use This Repository
1. Start with `organization/` for repository governance.
2. Apply `git/` standards for day-to-day delivery workflow.
3. Apply `architecture/` standards when defining APIs, service boundaries, and events.
4. Apply `code/` standards for language-level implementation and quality gates.
5. Apply `backend/`, `frontend/`, and `shared/` standards in implementation.
6. Use `templates/` for consistent repo docs, issues, PRs, and architecture decisions.

## Standard Format
Every standards document in this repository includes:
- Purpose
- Rules
- Allowed examples
- Disallowed examples
- Notes where needed

## Allowed Example
- `api-auth` repository links to:
  - `architecture/api-conventions.md` for response envelope rules
  - `backend/environment-variables.md` for runtime config
  - `git/pull-request-standard.md` for review requirements

## Disallowed Examples
- Service repository introducing a custom response envelope that conflicts with `architecture/api-conventions.md`.
- Team-specific commit format that bypasses `git/commit-convention.md`.
- Deployment manifest using mutable image tags contrary to `backend/docker-conventions.md`.

## Directory Map
- `organization/`: naming, ownership, governance metadata
- `git/`: branching, commits, PRs, and releases
- `architecture/`: service design, API contract standards, errors, audit events, pagination, filtering/sorting, idempotency, OpenAPI governance, rate limits, and deprecation policy
- `code/`: code quality, commenting style, Node.js, React, TypeScript, Go, Rust, and data-access standards
- `backend/`: runtime and operational service conventions
- `frontend/`: web dashboard architecture and auth-aware shell behavior
- `shared/`: shared package naming, boundaries, and error catalog governance
- `templates/`: standardized templates for day-to-day engineering workflows

## Change Control
Rules in this repository are normative.

Changes to standards must:
1. be submitted as pull requests
2. include rationale and migration impact
3. be reviewed by platform owners
4. include updates to templates when behavior changes
