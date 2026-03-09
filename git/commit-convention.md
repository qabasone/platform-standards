# Commit Convention Standard

## Purpose
Enforce a machine-parseable commit history that supports release automation, changelog generation, and fast code review.

## Rules
1. Commits must follow Conventional Commits:
   - `<type>(<scope>): <subject>`
2. Required `type` values:
   - `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `build`, `perf`, `revert`
3. `scope` is mandatory and should match service or module name (`api-auth`, `gateway`, `routing`, `policy`).
4. Subject must:
   - be imperative (`add`, `fix`, `remove`)
   - be lowercase except acronyms
   - not end with a period
   - be 72 characters or less
5. Breaking changes must include `BREAKING CHANGE:` in the commit body footer.
6. One commit should represent one logical change.

## Allowed Examples
- `feat(api-auth): add refresh token rotation`
- `fix(api-gateway): preserve trace_id on upstream errors`
- `docs(platform-standards): define audit event schema`
- `refactor(api-identity): split role assignment validator`

Example with breaking footer:
```text
feat(api-auth): rename token introspection endpoint

BREAKING CHANGE: /v1/auth/introspect moved to /v1/auth/token/introspect
```

## Disallowed Examples
- `updated stuff` (not conventional format)
- `feat: add login` (missing scope)
- `Fix(auth): Bug fix.` (improper casing and trailing period)
- `chore(api-auth):` (empty subject)

## Notes
- Squash merges must preserve a compliant final commit message.
- Auto-generated merge commits should be disabled unless required by policy.
