# Release Versioning Standard

## Purpose
Define a consistent release and tagging model for all services and applications.

## Rules
1. Versioning must follow Semantic Versioning: `MAJOR.MINOR.PATCH`.
2. Git tags must use `v` prefix:
   - `v1.0.0`
   - `v1.4.2`
3. Pre-release tags are allowed for staged validation:
   - `v1.5.0-rc.1`
4. Version bump policy:
   - `MAJOR`: incompatible API or behavior changes
   - `MINOR`: backward-compatible new functionality
   - `PATCH`: backward-compatible bug fixes
5. On merge to `main`, CI/CD must automatically:
   - create production release metadata
   - publish production artifact based on repository type (Docker image or npm package)
6. On merge to `dev`, CI/CD must automatically publish development artifact based on repository type (Docker image or npm package).
7. Docker publish rules:
   - `main` publishes tags: `latest` and `sha-<short-commit>`
   - `dev` publishes tags: `dev` and `sha-<short-commit>`
8. npm publish rules:
   - `main` publishes the SemVer version from `package.json` with dist-tag `latest`
   - `dev` publishes a dev build with dist-tag `dev`
9. Every release must include release notes summarizing:
   - key changes
   - migration actions
   - rollback guidance
10. Production deployments must reference immutable tags (`vX.Y.Z` or `sha-<commit>`), not branch names.
11. `latest` and `dev` tags are allowed as distribution channels, not as immutable deployment identifiers.

## Artifact Type Selection
1. If repository contains a service Docker build path (`Dockerfile`), publish Docker artifacts.
2. If repository is a publishable npm package, publish npm artifacts.
3. If repository intentionally produces both, both pipelines may run and must be independently versioned.

## Allowed Examples
- `v0.1.0` for first internal release
- `v0.2.0` for adding new auth endpoints without breaking changes
- `v0.2.1` for bug fix in token refresh
- `v1.0.0` when APIs are stabilized and publicly committed
- `main` Docker publish:
  - `ghcr.io/qabasone/api-auth:latest`
  - `ghcr.io/qabasone/api-auth:sha-9f2a7c1`
- `dev` Docker publish:
  - `ghcr.io/qabasone/api-auth:dev`
  - `ghcr.io/qabasone/api-auth:sha-a12b45d`
- `main` npm publish: `npm publish --tag latest`
- `dev` npm publish: `npm publish --tag dev`

## Disallowed Examples
- `1.2.3` (missing `v` prefix)
- `version-1.2.3` (invalid tag format)
- `v1.2` (incomplete SemVer)
- deploying `main` directly without a version tag
- merging to `main` without automatic release workflow
- publishing dev builds from `main`
- publishing production builds from `dev`

## Notes
- Tag creation must happen through CI or controlled release workflow.
- If a release includes breaking changes, link to ADR and migration documentation.
