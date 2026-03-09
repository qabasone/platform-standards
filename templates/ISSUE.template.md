# Issue Template Standard

## Purpose
Provide a structured issue template for consistent problem reporting, prioritization, and execution planning.

## Rules
1. Every issue must include clear problem statement and expected outcome.
2. Every issue must include impact and severity.
3. Reproduction steps are mandatory for bugs.
4. Acceptance criteria are mandatory for feature or improvement requests.
5. Required labels from organization standards must be applied after creation.

## Allowed Example
Use this content for issue creation template:

```markdown
## Issue Type
- [ ] Bug
- [ ] Feature
- [ ] Improvement
- [ ] Task

## Summary
<one clear sentence>

## Context
<why this matters now>

## Current Behavior
<what happens today>

## Expected Behavior
<what should happen>

## Impact
- Users affected: <who>
- Business impact: <low/medium/high>
- Operational risk: <low/medium/high>

## Reproduction Steps (Bug Only)
1. <step>
2. <step>
3. <result>

## Acceptance Criteria
- [ ] <criterion 1>
- [ ] <criterion 2>
- [ ] <criterion 3>

## Technical Notes
<known constraints, dependencies, related services>

## References
- Related PRs: <links>
- Related ADRs: <links>
- Related incidents: <links>
```

## Disallowed Examples
- Title-only issue with no body.
- "Does not work" reports without reproduction steps.
- Feature requests without acceptance criteria.
- Issues with no impact statement.

## Notes
- Keep issue content factual and implementation-oriented.
- Discussion and decisions that change architecture must be captured in ADRs.
