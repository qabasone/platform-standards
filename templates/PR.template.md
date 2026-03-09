# Pull Request Template Standard

## Purpose
Provide a mandatory pull request template so reviewers can evaluate risk, validation, and rollout readiness consistently.

## Rules
1. Every pull request must complete all sections in this template.
2. Empty sections are not allowed; use `N/A` only when truly not applicable.
3. Testing evidence is mandatory for behavior changes.
4. Rollout and rollback notes are mandatory for production-impacting changes.

## Allowed Example
Use this content in `.github/pull_request_template.md`:

```markdown
## Summary
<what changed and why>

## Linked Work
- Issue: #<id>
- Tracker: <QAB-123>

## Change Type
- [ ] feat
- [ ] fix
- [ ] refactor
- [ ] docs
- [ ] chore

## Scope
- [ ] api-gateway
- [ ] api-auth
- [ ] api-identity
- [ ] web-dashboard
- [ ] shared package
- [ ] other: <name>

## Implementation Notes
- <key implementation detail 1>
- <key implementation detail 2>

## Testing Evidence
- Unit tests: <added/updated + command output summary>
- Integration tests: <added/updated + command output summary>
- Manual verification: <steps and result>

## Security And Data Impact
- [ ] no security impact
- [ ] security-impacting change described below

Details:
<auth, permission, secret, audit, or data-handling changes>

## Rollout Plan
1. <step>
2. <step>

## Rollback Plan
1. <step>
2. <step>

## Checklist
- [ ] commit messages follow convention
- [ ] docs updated
- [ ] API contract unchanged or versioned
- [ ] required API CI checks passed (`openapi-validate`, `openapi-lint`, `openapi-breaking-change-check`, `docs-response-contract-lint`) when applicable
- [ ] required stack quality checks passed (`lint`, `format-check`, `typecheck`, `test`, plus stack-specific checks)
- [ ] error codes/message keys updated in `en` and `ar-EG` catalogs (if applicable)
- [ ] audit events considered
- [ ] reviewers added from CODEOWNERS
```

## Disallowed Examples
- PR with only title and no description.
- Missing testing evidence for code changes.
- Missing rollout/rollback details for risky changes.
- Checklist auto-marked without verification.

## Notes
- Teams may extend the template, but required sections must remain.
