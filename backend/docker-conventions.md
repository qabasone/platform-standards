# Docker Conventions Standard

## Purpose
Define image, container, and build conventions to ensure consistent packaging and deployment across services.

## Rules
1. Docker image names must follow:
   - `ghcr.io/qabasone/<service-name>`
2. Container names must follow:
   - `qabas-<service-name>`
3. Service name in image and container must match canonical service name.
4. Dockerfiles must use multi-stage builds for production images.
5. Runtime containers must run as non-root user.
6. CI/CD tagging policy:
   - merge to `main`: publish `latest` and `sha-<short-commit>`
   - merge to `dev`: publish `dev` and `sha-<short-commit>`
7. Deployment manifests must reference immutable tags (`vX.Y.Z` or `sha-<commit>`), not mutable tags.
8. Base images must be pinned to explicit versions.
9. Image labels must include source and revision metadata.
10. One Docker image must run one process (except init wrappers).
11. Production image pipeline must include security checks before publish:
   - dependency vulnerability scan
   - base image vulnerability scan
   - secret detection in build context
12. Critical or high vulnerabilities in runtime layers must fail the production image publish workflow.
13. Production image must include only required runtime dependencies (no build tools in final stage).

## Allowed Examples
Images:
- `ghcr.io/qabasone/api-auth`
- `ghcr.io/qabasone/api-gateway`
- `ghcr.io/qabasone/api-identity`

Containers:
- `qabas-api-auth`
- `qabas-api-gateway`
- `qabas-api-identity`

Example image tags:
- `ghcr.io/qabasone/api-auth:v0.3.1`
- `ghcr.io/qabasone/api-auth:latest`
- `ghcr.io/qabasone/api-auth:dev`
- `ghcr.io/qabasone/api-auth:sha-9f2a7c1`

Pull commands:
```bash
docker pull ghcr.io/qabasone/api-auth:latest
docker pull ghcr.io/qabasone/api-auth:dev
docker pull ghcr.io/qabasone/api-auth:sha-9f2a7c1
```

## Disallowed Examples
- `docker.io/qabasone/api-auth` (wrong registry)
- `ghcr.io/qabasone/AuthService` (invalid service naming)
- `qabas_auth_service` (invalid container naming format)
- publishing `latest` from `dev`
- publishing `dev` from `main`
- Dockerfile running as root user in production stage
- shipping production image with unresolved critical vulnerabilities

## Notes
- Build cache strategy and multi-arch settings are repository-specific and can be defined in CI workflow files.
- If sidecars are required, each sidecar follows the same naming and tagging conventions.
