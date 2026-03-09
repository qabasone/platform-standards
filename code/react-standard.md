# React Frontend Standard

## Purpose
Define React implementation standards for maintainable, secure, and performant frontend applications.

## Rules
1. Use functional components and hooks; class components are forbidden for new code.
2. Keep components presentational by default; move domain logic to feature services/hooks.
3. Side effects must be handled in hooks, not during render.
4. Route-level code splitting is required for major pages.
5. UI authorization checks must align with route-level authorization checks.
6. Accessibility basics are mandatory (`label`, keyboard support, semantic elements).
7. Avoid prop drilling for global app concerns; use scoped context or state management.
8. API calls must go through shared service layer, not directly from random components.
9. Error and loading states must be explicitly rendered.

## Allowed Examples
- `useEffect` for API fetch with cleanup.
- Dedicated `features/users` hooks handling business behavior.
- `ProtectedRoute` + permission-aware UI controls.

## Disallowed Examples
- Data fetch in render body.
- Component writing raw fetch logic duplicated across pages.
- Hidden button as only auth control while route is still unprotected.

## Notes
- Follow frontend routing and auth-aware shell standards in this repository.
- Keep component files small and focused.