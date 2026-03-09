# Repository Structure Standard

## Purpose
Define a consistent baseline directory and file structure for repositories so onboarding, automation, and maintenance stay predictable.

## Rules
1. Every repository must include at root:
   - `README.md`
   - `.gitignore`
   - `LICENSE` (or organization-approved alternative)
   - `.github/` for automation and templates
2. Service repositories must include:
   - `src/` for implementation code
   - `tests/` for automated tests
   - `docs/` for service-specific operational and architecture documents
3. `docs/adr/` must exist when architectural decisions are tracked in the repository.
4. CI workflows must be stored in `.github/workflows/`.
5. Generated artifacts (`dist/`, compiled assets, build outputs) must not be committed unless explicitly approved for static distribution.
6. Configuration files must be at root and explicitly named (`docker-compose.yml`, `tsconfig.json`, `eslint.config.js`, `Makefile`).
7. One repository must represent one deployable system or one shared package family, not both.

## Allowed Examples
```text
api-auth/
  README.md
  .gitignore
  .github/
    workflows/
      ci.yml
    CODEOWNERS
  src/
  tests/
  docs/
    adr/
  Dockerfile
```

```text
web-dashboard/
  README.md
  src/
    app/
    features/
  tests/
  public/
  .github/
```

## Disallowed Examples
```text
api-identity/
  source/
  test/
```
Reason: non-standard folder names and missing required root files.

```text
api-gateway/
  src/
  dist/
  node_modules/
```
Reason: generated artifacts committed to the repository.

```text
platform-mixed/
  api/
  web/
  infra/
```
Reason: multiple unrelated deployables in one repository.

## Notes
- This `platform-standards` repository is documentation-focused and intentionally does not include `src/` or `tests/`.
- Exceptions must be approved in writing by platform owners and documented in the repository README.
