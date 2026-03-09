# ADR Template Standard

## Purpose
Provide a standard Architecture Decision Record format so architectural changes are explicit, reviewable, and traceable.

## Rules
1. Every architecture-impacting decision must have an ADR before implementation.
2. ADR filename format:
   - `YYYYMMDD-short-title.md`
3. Status must be one of:
   - `proposed`
   - `accepted`
   - `rejected`
   - `superseded`
4. ADR must document context, options, decision, and consequences.
5. Superseded ADRs must link to replacement ADR.

## Allowed Example
Use this structure for each ADR file:

```markdown
# ADR: <decision title>

## Metadata
- Date: <YYYY-MM-DD>
- Status: <proposed|accepted|rejected|superseded>
- Owners: <team or names>
- Related Issue: <link>
- Related PR: <link>

## Context
<facts and constraints that require a decision>

## Decision Drivers
- <driver 1>
- <driver 2>
- <driver 3>

## Considered Options
1. <option 1>
2. <option 2>
3. <option 3>

## Decision
<chosen option and why>

## Consequences
### Positive
- <impact>

### Negative
- <tradeoff>

### Risks
- <risk and mitigation>

## Rollout Plan
1. <step>
2. <step>

## Validation
- <how decision success will be measured>
```

## Disallowed Examples
- ADR without status.
- ADR that states a decision with no alternatives considered.
- ADR with no consequences section.
- Architecture change merged with no ADR when required.

## Notes
- Keep ADRs short and factual.
- Update ADR status if decision direction changes.
