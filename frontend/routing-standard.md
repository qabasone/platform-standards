# Frontend Routing Standard

## Purpose
Define mandatory route structure and protection behavior for the `qabas.one` web dashboard.

## Rules
1. The application must provide these routes:
   - `/login`
   - `/`
   - `/users`
   - `/settings`
   - `/unauthorized`
   - `/404`
2. Route categories:
   - Public: `/login`, `/404`
   - Authenticated: `/`, `/users`, `/settings`
   - Authenticated but forbidden: redirect to `/unauthorized`
3. Unknown paths must render `/404`.
4. Protected routes must enforce both:
   - authenticated session
   - permission check when required
5. Authenticated users visiting `/login` should be redirected to `/`.
6. Route-level permission metadata must be explicit and machine-readable.

## Allowed Examples
```ts
const routes = [
  { path: "/login", access: "public" },
  { path: "/", access: "authenticated" },
  { path: "/users", access: "permission", permission: "users.read" },
  { path: "/settings", access: "permission", permission: "settings.read" },
  { path: "/unauthorized", access: "public" },
  { path: "/404", access: "public" }
];
```

Guard behavior:
- unauthenticated access to `/users` redirects to `/login`
- authenticated user without `users.read` redirects to `/unauthorized`
- unknown route redirects to `/404`

## Disallowed Examples
- Missing `/unauthorized` route.
- Returning blank page for unknown routes instead of `/404`.
- Checking permissions only in UI components while leaving route unguarded.
- Allowing `/settings` without authentication guard.

## Notes
- Route permissions should align with backend policy actions to avoid drift.
- Navigation visibility and route access checks must use the same permission source.
