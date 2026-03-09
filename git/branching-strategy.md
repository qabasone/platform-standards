# Branching Strategy Standard

## Purpose
Define a deterministic Git workflow for all repositories to reduce integration risk and keep delivery fast.

## Rules
1. Every repository must have exactly two long-lived branches:
   - `main` (production branch)
   - `dev` (integration branch)
2. Direct pushes to `main` and `dev` are forbidden; all changes must go through pull requests.
3. Use short-lived branches with this format:
   - `feat/<service>/<ticket>-<slug>`
   - `fix/<service>/<ticket>-<slug>`
   - `chore/<service>/<ticket>-<slug>`
   - `hotfix/<service>/<ticket>-<slug>`
4. Day-to-day feature and bug branches must be created from `dev` and merged back into `dev`.
5. Promotion to production must happen by pull request from `dev` to `main`.
6. Branch lifetime should not exceed 5 working days.
7. Rebase from the target base branch (`dev` or `main`) before opening or updating a pull request.
8. `hotfix/*` branches are allowed only for production incidents, must be created from `main`, and must be merged back into both `main` and `dev`.
9. Release artifacts for production are created from `main`; development artifacts are created from `dev`.

## Branch Workflow Matrix
| Branch | Role | Allowed PR Sources | CI/CD Outcome |
|---|---|---|---|
| `dev` | integration and validation | `feat/*`, `fix/*`, `chore/*`, `hotfix/*` | build and publish `dev` channel artifact |
| `main` | production | `dev`, `hotfix/*` | build and publish production artifact and release |

## Allowed Examples
- `feat/api-auth/QAB-231-add-refresh-rotation`
- `fix/api-gateway/QAB-388-rate-limit-header`
- `chore/web-dashboard/QAB-402-eslint-cleanup`
- `hotfix/api-identity/QAB-455-login-timeout`
- PR flow: `feat/api-auth/QAB-231-add-refresh-rotation` -> `dev`
- PR flow: `dev` -> `main` for production release promotion

## Disallowed Examples
- `feature/new-auth` (wrong prefix and missing service/ticket)
- `fix-login` (missing structure)
- `release/v1` (long-lived release branch pattern is forbidden)
- `mahros/experiment` (personal branch naming)
- committing directly to `main`
- committing directly to `dev`
- merging `feat/*` directly into `main` without `dev` promotion path

## Notes
- Branch protection on `main` and `dev` must require passing CI and required approvals.
- Emergency changes still require post-incident pull request documentation.
