# Auth-Aware Web Shell Standard

## Purpose
Define required behavior for the `qabas.one` web application shell so authentication and authorization are enforced consistently at runtime.

## Rules
1. The shell must load authentication state before rendering protected routes.
2. Session state must include:
   - `is_authenticated`
   - `user_id`
   - `roles`
   - `permissions`
   - token expiration metadata
3. Protected routes must render only after auth bootstrap finishes.
4. The shell must support automatic access token refresh before expiry.
5. Refresh failures must trigger immediate session invalidation and redirect to `/login`.
6. Permission checks must run at route level and component level for critical actions.
7. Unauthorized access must route to `/unauthorized`, not `/404`.
8. Refresh tokens must never be accessible from JavaScript if httpOnly cookie strategy is available.
9. Auth state changes (login, logout, refresh failure) must emit structured analytics/audit hooks.

## Allowed Examples
Startup flow:
1. App starts.
2. Shell checks existing session.
3. If valid, load user identity and permissions.
4. Render route tree.
5. Schedule token refresh.

Token refresh behavior:
- refresh at 70-80% of access token lifetime
- single in-flight refresh request at a time
- queue pending API calls until refresh resolves

Permission gate example:
```ts
if (!auth.isAuthenticated) return redirect("/login");
if (!auth.permissions.includes("users.read")) return redirect("/unauthorized");
```

## Disallowed Examples
- Rendering protected pages before auth bootstrap completes.
- Storing refresh token in `localStorage`.
- Silently failing token refresh and keeping stale authenticated UI.
- Using only menu visibility as authorization control without route guards.

## Notes
- This shell contract is mandatory for the dashboard foundation and all future web modules.
- If SSO is added later, the same auth-aware behavior still applies at route and permission layers.
