# Repository README Template Standard

## Purpose
Provide a mandatory README template for all engineering repositories so service purpose, ownership, and operations are always documented.

## Rules
1. Every repository README must include all sections in this template.
2. Replace bracketed placeholders with real values.
3. Keep examples and commands executable for the current repository.
4. Keep ownership and runbook links current.

## Allowed Example
Use this content as the repository `README.md` baseline:

````markdown
# <repository-name>

## Purpose
<one-paragraph summary of what this repository provides>

## Scope
- <in-scope capability 1>
- <in-scope capability 2>
- <out-of-scope statement>

## Ownership
- Primary team: <github-team>
- Backup team: <github-team>
- Escalation channel: <slack-or-oncall-channel>

## Architecture
- Service name: <service-name>
- Runtime: <runtime and framework>
- Dependencies: <database, cache, queue, external APIs>
- Related ADRs: <links>

## API Surface (if applicable)
- Base path: `/v1/...`
- Auth model: <jwt/session/internal>
- Key endpoints:
  - `<METHOD> <path>`
  - `<METHOD> <path>`

## Local Development
```bash
<install-command>
<run-command>
<test-command>
```

## Environment Variables
- See: `<link-to-env-doc-or-table>`

## Deployment
- Image: `ghcr.io/qabasone/<service-name>`
- Health endpoints:
  - `/health/live`
  - `/health/ready`

## Observability
- Logs: structured JSON with `trace_id`
- Metrics: <link or summary>
- Audit events: <link or summary>

## Runbook
- Common failures: <link>
- Rollback steps: <link>
````

## Disallowed Examples
- README with only project name and no operational content.
- Missing ownership information.
- Stale commands that do not run.
- Placeholder values committed unchanged.

## Notes
- Repositories may add sections, but required sections must remain.
