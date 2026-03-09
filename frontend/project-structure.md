# Frontend Project Structure Standard

## Purpose
Define a scalable project layout for the platform web dashboard so auth, routing, and feature modules remain maintainable.

## Rules
1. Frontend app repositories must use feature-oriented structure under `src/`.
2. Required top-level frontend folders:
   - `src/app/` (application bootstrap, providers, shell)
   - `src/routes/` (route definitions and guards)
   - `src/features/` (domain features)
   - `src/shared/` (shared UI primitives and utilities)
   - `src/services/` (API clients and adapters)
   - `src/auth/` (session state, token handling, permission checks)
3. Business logic must stay inside feature modules, not global shared folders.
4. Feature modules must not import from other feature internals.
5. UI components in `shared/` must be generic and reusable; no domain-specific behavior.
6. Page-level routes must map to one feature entry point.

## Allowed Examples
```text
src/
  app/
    bootstrap.tsx
    shell/
  auth/
    auth-store.ts
    token-refresh.ts
    permission-guard.tsx
  routes/
    index.tsx
    protected-route.tsx
  features/
    users/
      pages/
      components/
      api/
    settings/
      pages/
      components/
      api/
  services/
    http-client.ts
  shared/
    ui/
    utils/
```

Allowed import:
```ts
import { fetchUsers } from "@/features/users/api/fetch-users";
```

## Disallowed Examples
```text
src/
  components/
  pages/
  utils/
```
Reason: generic flat layout does not scale with feature boundaries.

Disallowed import:
```ts
import { internalMapper } from "@/features/users/internal/mapper";
```
From another feature module.

## Notes
- Use path aliases to prevent brittle relative imports.
- Shared styles and design tokens should live under `src/shared/` and not under individual features.
