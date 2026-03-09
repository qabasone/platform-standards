# Deprecation Policy Standard

## Purpose
Define a predictable lifecycle for API deprecation and removal.

## Rules
1. API deprecations must be announced before removal.
2. Deprecated endpoints must return `Deprecation: true` header.
3. Deprecated endpoints must return `Sunset` header with RFC 1123 date.
4. Deprecated endpoints should include `Link` header to migration documentation with `rel="deprecation"`.
5. Minimum deprecation window for stable APIs is `90` days.
6. Removal before the deprecation window is forbidden unless a security emergency is approved.
7. Breaking removals require:
   - SemVer major bump
   - ADR
   - migration runbook
8. Deprecation notices must be reflected in:
   - OpenAPI (`deprecated: true`)
   - release notes
   - consumer communication channel
9. New replacement endpoint/version must be documented before deprecating old one.

## Lifecycle Stages
1. Announced:
   - endpoint active
   - deprecation communication published
2. Deprecated:
   - endpoint active
   - deprecation headers present
   - migration path published
3. Sunset reached:
   - endpoint removed or disabled
   - clear standardized error returned

## Allowed Examples
Deprecated response headers:
```http
Deprecation: true
Sunset: Tue, 30 Jun 2026 00:00:00 GMT
Link: <https://docs.qabas.one/migrations/auth-v2>; rel="deprecation"
```

OpenAPI deprecation flag:
```yaml
paths:
  /v1/auth/legacy-login:
    post:
      deprecated: true
```

## Disallowed Examples
- Removing endpoint with no prior deprecation window.
- Setting `Deprecation: true` but omitting `Sunset` header.
- Deprecating endpoint with no migration link.
- Breaking consumer integrations without release notes and ADR.

## Notes
- Internal-only endpoints may use shorter windows with platform owner approval.
- Security vulnerabilities may trigger emergency timelines; decision must be documented.