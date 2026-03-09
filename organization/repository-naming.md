# Repository Naming Standard

## Purpose
Define a strict, predictable naming scheme for repositories in `github.com/qabasone` so services and tooling can discover intent from repository names.

## Rules
1. Repository names must be lowercase `kebab-case`.
2. Repository names must start with one approved prefix:
   - `api-` for backend API services
   - `web-` for frontend applications
   - `lib-` for reusable runtime libraries
   - `infra-` for infrastructure-as-code repositories
   - `ops-` for operational automation and runbooks
   - `platform-` for organization-wide standards or platform governance
3. Names must describe one bounded context or capability.
4. Names must not include environment suffixes (`-dev`, `-staging`, `-prod`).
5. Names must not include personal, experimental, or temporary wording (`-test`, `-new`, `-v2-temp`).
6. Maximum length is 40 characters.

## Allowed Examples
- `api-gateway`
- `api-auth`
- `api-identity`
- `web-dashboard`
- `platform-standards`
- `infra-cluster-bootstrap`

## Disallowed Examples
- `API-Auth` (uppercase characters)
- `api_auth` (underscore usage)
- `gateway-service` (missing approved prefix)
- `api-auth-prod` (environment embedded in name)
- `mahros-auth-service` (personal name in repository)
- `api-auth-test` (temporary naming)

## Notes
- Existing repositories that violate this standard must be renamed at the next planned breaking maintenance window.
- Service runtime environment should be represented in deployment metadata, not in repository name.
