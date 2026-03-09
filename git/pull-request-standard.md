# Pull Request Standard

## Purpose
Define minimum quality and review requirements for pull requests before code merges to `dev` or `main`.

## Rules
1. Every pull request must use the repository PR template.
2. PR title must follow commit convention format:
   - `<type>(<scope>): <summary>`
3. PR description must include:
   - problem statement
   - summary of changes
   - test evidence
   - rollout and rollback notes
4. Every PR must reference at least one issue (`Closes #<number>` or equivalent tracker ID).
5. CI checks must pass before merge.
6. API-related pull requests must pass required contract checks:
   - `openapi-validate`
   - `openapi-lint`
   - `openapi-breaking-change-check`
   - `docs-response-contract-lint`
7. Stack-specific quality checks must pass before merge:
   - Node.js/TypeScript: `lint`, `format-check`, `typecheck`, `test`
   - React/TypeScript: `lint`, `format-check`, `typecheck`, `test`, `build`
   - Go: `gofmt-check`, `go vet`, `go test ./...`
   - Rust: `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test`
8. Required approvals:
   - minimum one approval from code owner for normal changes
   - minimum two approvals for high-risk or auth/policy/security changes
9. PRs larger than 600 changed lines require explicit reviewer agreement or decomposition.
10. Merge strategy is `Squash and merge` unless repository policy requires otherwise.
11. Pull request target branch rules:
   - feature/fix/chore branches target `dev`
   - promotion pull requests target `main` from `dev`
   - hotfix branches may target `main` and must be back-merged to `dev`

## Allowed Examples
Title:
`feat(api-identity): add branch assignment endpoint`

Description summary:
```text
Problem:
Users could not be assigned to branches through API.

Changes:
- Added POST /v1/users/{id}/branches
- Added validator and authorization checks
- Added audit event ROLE_ASSIGNED

Testing:
- Unit tests for validator and service
- Integration tests for endpoint and permissions

Rollout:
- Deploy api-identity first
- Enable UI control after backend deploy
```

## Disallowed Examples
- Title: `Update code` (not compliant format)
- Empty PR body
- Missing linked issue
- Merging with failing CI
- Self-approval when code owner approval is required
- Direct feature PR to `main` without approved exception
- API contract changes merged without `docs-response-contract-lint` pass
- Stack changes merged without mandatory quality check pass

## Notes
- If a PR intentionally deviates from standards, include an explicit exception section and approval from platform owners.
