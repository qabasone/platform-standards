# Service Boundaries Standard

## Purpose
Enforce strict bounded contexts between platform services to prevent coupling, duplicated logic, and unclear ownership.

## Rules
1. `api-gateway` owns only:
   - request routing
   - authentication verification
   - API aggregation
   - rate limiting
2. `api-auth` owns only:
   - login and logout
   - session lifecycle management
   - credential validation
   - access and refresh token issuance/rotation/revocation
3. `api-identity` owns only:
   - user profile data
   - role assignment
   - branch assignment
   - verification status
   - user metadata
4. Service responsibilities must not overlap.
5. Services must not read or write another service's database directly.
6. Cross-service interactions must happen through explicit APIs or approved event contracts.
7. `api-gateway` must remain stateless for business data and must not become a domain service.
8. Authorization policy decisions must be delegated to policy components (`policy engine` when available) and not duplicated across services.
9. Localization of user-facing error messages must be centralized in one edge layer:
   - `api-gateway`, or
   - BFF, or
   - shared localization layer used by edge services
10. Domain services (`api-auth`, `api-identity`, and future services) must return structured error metadata (`error_code`, `message_key`, `params`, `http_status`, `trace_id`) instead of localized user strings.
11. `developer_message` is internal diagnostics and must not cross service boundaries toward public clients.

## Responsibility Matrix
| Service | Owns | Must Not Own |
|---|---|---|
| `api-gateway` | routing, auth verification, aggregation, rate limits, edge localization resolution | login credentials, token issuance, user profiles, role storage |
| `api-auth` | credential validation, sessions, tokens, structured error metadata | profile edits, branch assignment, role persistence, localized user messaging |
| `api-identity` | user profile, roles, branches, metadata, structured error metadata | login flow, token issuance, password validation, localized user messaging |

## Allowed Examples
- `POST /v1/auth/login` implemented in `api-auth`.
- `PATCH /v1/users/{id}/profile` implemented in `api-identity`.
- `api-gateway` validates token signature and forwards authorized requests to downstream services.
- `api-gateway` calls both `api-auth` and `api-identity` for aggregated `GET /v1/me` response.
- `api-auth` returns `message_key=auth.invalid_credentials`; gateway returns localized `error.message` based on locale.

## Disallowed Examples
- `api-gateway` implementing password verification.
- `api-auth` storing role assignment tables.
- `api-identity` issuing JWT tokens.
- `api-auth` writing directly to `api-identity` database tables.
- `api-identity` returning hardcoded Arabic or English user-facing error text.
- downstream service exposing `developer_message` directly to client payload.

## Notes
- Any boundary change must be documented in an ADR before implementation.
- Boundary violations are blocking review findings and must be fixed before merge.
