# Commenting Standard

## Purpose
Define consistent commenting style that improves maintainability without cluttering code.

## Rules
1. Comments must explain intent, constraints, or tradeoffs; avoid restating obvious code.
2. Multi-line comments must be on dedicated lines above the code block they describe.
3. Inline comments (`// ...` at end of code line) are forbidden by default.
4. Inline comments are allowed only for:
   - linter directives
   - short machine-readable markers required by tooling
5. TODO comments must include tracker ID and owner.
6. Temporary comments for debugging must not be merged.
7. Public exported modules/functions should include concise doc comments when behavior is non-trivial.

## Allowed Examples
Dedicated-line comment:
```ts
// Reject stale tokens to avoid replay in concurrent refresh calls.
if (tokenIssuedAt < sessionRotatedAt) {
  return false;
}
```

TODO with ownership:
```ts
// TODO(QAB-482, @identity-team): move this mapper to shared package after policy review.
```

## Disallowed Examples
Inline comment style:
```ts
const ttl = 900; // access token ttl
```
Reason: inline explanatory comments are not allowed.

Redundant comment:
```ts
// Increment i by 1
i++;
```
Reason: comment adds no value.

Commented-out dead code:
```ts
// return legacyValidation(payload);
```
Reason: remove dead code; rely on Git history.

## Notes
- This standard applies to all languages in this platform.
- If a repository needs explicit exceptions, document them in its README.