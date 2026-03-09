# Service Naming Standard

## Purpose
Define deterministic service naming across code, deployment metadata, logging, and environment configuration.

## Rules
1. Service names must be lowercase `kebab-case`.
2. API services must use `api-` prefix.
3. Frontend applications must use `web-` prefix.
4. Worker/background processors must use `worker-` prefix.
5. `SERVICE_NAME` environment variable must match the canonical service name exactly.
6. Service name must be consistent across:
   - repository name
   - deployment manifest
   - container name suffix
   - log `service_name` field
7. Service aliases are forbidden in production configuration.

## Allowed Examples
- `api-gateway`
- `api-auth`
- `api-identity`
- `web-dashboard`
- `worker-audit-sync`

## Disallowed Examples
- `APIGateway` (not kebab-case)
- `gateway-api` (wrong prefix order)
- `auth-service` (non-standard prefix)
- `api_auth` (underscore)

## Notes
- Current canonical service set:
  - `api-gateway`
  - `api-auth`
  - `api-identity`
- Future policy component should be named using this standard (for example: `api-policy` or `worker-policy-evaluator` based on runtime role).
