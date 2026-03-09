# Code Quality Standard

## Purpose
Define minimum code quality gates for all repositories across the platform stack.

## Rules
1. Every repository must declare its stack profile (`node-ts`, `react-ts`, `go`, `rust`, or hybrid).
2. Every pull request must pass mandatory quality checks for its stack profile.
3. Linting, formatting, type checking, and tests must run in CI before merge.
4. Failing quality checks block merge to `dev` and `main`.
5. Test coverage minimum is `80%` for business-critical modules unless an approved exception exists.
6. New public APIs and shared libraries must include tests in the same pull request.
7. Security checks must run for dependency vulnerabilities.
8. Quality tooling must be deterministic and pinned by version in repository config.

## Required Checks By Stack
- Node.js/TypeScript backend:
  - `lint`
  - `format-check`
  - `typecheck`
  - `test`
- React/TypeScript frontend:
  - `lint`
  - `format-check`
  - `typecheck`
  - `test`
  - `build`
- Go services:
  - `gofmt -w` (local) and format check in CI
  - `go vet`
  - `go test ./...`
- Rust services:
  - `cargo fmt --check`
  - `cargo clippy -- -D warnings`
  - `cargo test`

## Allowed Examples
- PR merges only after all quality checks are green.
- Repository defines scripts/targets for all mandatory checks.
- Coverage exception documented in PR with owner approval.

## Disallowed Examples
- Skipping tests because change is "small".
- Merging with lint warnings unresolved.
- Running type checks only locally and not in CI.
- Introducing new module without tests.

## Notes
- Teams may add stricter rules, but not weaker ones.
- Emergency production fixes still require post-merge quality follow-up PR.