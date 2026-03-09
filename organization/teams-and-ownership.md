# Teams And Ownership Standard

## Purpose
Define mandatory ownership rules so every repository and critical code path has clear accountability for delivery, operations, and incident response.

## Rules
1. Every repository must have one primary owning team and at least one backup reviewing team.
2. Ownership must be declared in `.github/CODEOWNERS`.
3. Pull requests touching production-impacting paths must include at least one approval from the primary owner team.
4. Changes in `security`, `auth`, and `policy` areas require two approvals:
   - one from owning team
   - one from `@qabasone/platform-core` or designated security reviewers
5. Each repository README must include:
   - primary team
   - on-call escalation channel
   - service criticality level
6. No repository may remain without active owner assignment.

## Allowed Examples
```text
# .github/CODEOWNERS
*                                  @qabasone/platform-core
/src/auth/**                       @qabasone/identity-access
/src/policy/**                     @qabasone/identity-access @qabasone/platform-core
/docs/**                           @qabasone/platform-core
```

```text
Primary owner: @qabasone/identity-access
Backup reviewers: @qabasone/platform-core
Escalation: #platform-oncall
```

## Disallowed Examples
```text
# .github/CODEOWNERS
* @individual-user
```
Reason: ownership cannot depend on one person.

```text
# .github/CODEOWNERS
```
Reason: empty ownership is not allowed.

```text
Primary owner: TBD
```
Reason: undefined ownership is not allowed.

## Notes
- Team names must map to actual GitHub teams in `github.com/qabasone`.
- Ownership changes must be reviewed by both outgoing and incoming owners before merge.
