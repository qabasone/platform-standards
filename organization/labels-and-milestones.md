# Labels And Milestones Standard

## Purpose
Define a consistent metadata model for issues and pull requests so planning, automation, and reporting are reliable across all repositories.

## Rules
1. Every issue must have exactly:
   - one `type:*` label
   - one `priority:*` label
2. Every issue should have one `area:*` label when scope is known.
3. Use only approved label namespaces:
   - `type:` (`type:feature`, `type:bug`, `type:docs`, `type:chore`, `type:incident`)
   - `area:` (`area:gateway`, `area:auth`, `area:identity`, `area:web`, `area:policy`, `area:platform`)
   - `priority:` (`priority:p0`, `priority:p1`, `priority:p2`, `priority:p3`)
   - `status:` (`status:blocked`, `status:needs-info`, `status:ready`)
   - `risk:` (`risk:high`, `risk:medium`, `risk:low`)
4. Labels must be lowercase and use `:` separator.
5. Milestones must follow format: `YYYY.MM - <release-or-goal>`.
6. PRs must inherit issue labels where applicable.
7. `priority:p0` issues must always have a milestone and owner.

## Allowed Examples
- Labels:
  - `type:feature`
  - `area:auth`
  - `priority:p1`
  - `status:ready`
- Milestones:
  - `2026.03 - auth-foundation`
  - `2026.04 - gateway-hardening`

## Disallowed Examples
- `bug` (missing namespace)
- `Priority-High` (wrong format and casing)
- `type:feature` and `type:bug` on same issue (multiple type labels)
- `March Sprint` (milestone format mismatch)
- unlabeled issue with no priority

## Notes
- Automation may reject issues and PRs that do not match this standard.
- Label color values are organization-managed and are not part of this specification.
