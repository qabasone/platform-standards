# TypeScript Standard

## Purpose
Define safe, predictable TypeScript coding standards for backend and frontend projects.

## Rules
1. `strict` mode must be enabled in `tsconfig`.
2. `any` is forbidden unless explicitly justified with a lint override and reason.
3. Prefer `unknown` over `any` for external/untrusted input.
4. All external input must be validated at runtime (for example Zod/Joi) before use.
5. Use explicit return types for exported functions.
6. Prefer union and literal types over numeric/string enums when practical.
7. Use `readonly` for immutable data contracts.
8. Avoid default exports for shared libraries; prefer named exports.
9. Compile-time type checks must pass in CI (`tsc --noEmit`).

## Allowed Examples
```ts
type UserRole = "admin" | "operator" | "viewer";

export function canAssignRole(role: UserRole): boolean {
  return role !== "viewer";
}
```

```ts
const parsed = userSchema.parse(payload);
```

## Disallowed Examples
```ts
export function mapUser(input: any) {
  return input.profile.name;
}
```
Reason: unsafe `any` and no validation.

```ts
export default function helper() {}
```
Reason: default export discouraged for shared modules.

## Notes
- Type safety does not replace runtime validation for API boundaries.
- Keep domain types near domain modules; move only stable contracts to shared packages.