# Shared Boundaries Standard

## Purpose
Prevent architecture erosion by defining exactly what shared packages may and may not contain.

## Rules
1. Shared packages must remain framework-agnostic and domain-light.
2. Allowed shared content:
   - types
   - interfaces
   - validation schemas
   - configuration contracts
   - error code and message key definitions
   - localization message catalogs (`en`, `ar-EG`)
   - placeholder interpolation utilities
3. Forbidden shared content:
   - database access
   - controllers
   - business services
4. Shared packages must not call external services directly.
5. Shared packages must not depend on service runtime containers (database clients, web frameworks, queue consumers).
6. Shared utilities must be deterministic and side-effect controlled.
7. Business logic belongs in owning services (`api-auth`, `api-identity`, etc.), not in shared packages.
8. Known error translations must be catalog-driven (static keys/files), not generated dynamically per request.
9. LLM usage is allowed only for offline drafting/review of catalog entries, not as runtime source of truth for known errors.
10. Shared localization packages must preserve placeholder names exactly across locales (`{required}`, `{currency}`, `{available}`).

## Allowed Examples
- `@qabasone/types-auth` exporting `AccessTokenClaims` interface
- `@qabasone/schemas-identity` exporting Zod/Joi schemas for user payload validation
- `@qabasone/config-env` exporting environment variable parsing contract
- `@qabasone/i18n-errors` exporting `en.json` and `ar-EG.json` message catalogs
- shared helper that formats `wallet.insufficient_balance` using provided `params`

## Disallowed Examples
- shared package opening MongoDB connections
- shared package containing `UserController` classes
- shared package implementing `assignRoleToUser` business workflow
- shared package importing `express`, `nestjs`, or database ORM client
- runtime LLM call for every known error message translation
- locale file changing placeholder keys between languages

## Notes
- If logic starts requiring service dependencies or domain policies, move it into the owning service.
- Boundary violations in shared packages are blocking and must be refactored before release.
- Error key lifecycle and localization ownership rules are defined in `shared/error-catalog-governance.md`.
