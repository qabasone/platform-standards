# OpenAPI Governance Standard

## Purpose
Ensure API contracts are versioned, validated, and backward-compatibility checked in CI.

## Rules
1. Every API service repository must keep a canonical OpenAPI document at `openapi/openapi.yaml`.
2. OpenAPI must define all public and internal service endpoints exposed by that repository.
3. OpenAPI must include schema examples that follow platform response envelopes.
4. CI must validate OpenAPI syntax and schema correctness on every pull request.
5. CI must run OpenAPI lint rules (naming, operation IDs, tags, response schema completeness).
6. CI must run breaking-change detection against the target base branch (`dev` or `main`).
7. Breaking API changes require:
   - SemVer major bump (or approved pre-1.0 policy)
   - ADR link
   - migration notes in PR
8. Merge is blocked when OpenAPI CI checks fail.
9. Generated SDKs/clients must be generated from the canonical OpenAPI contract, not handcrafted endpoints.
10. OpenAPI operation examples must include `trace_id` and standardized error fields.
11. CI must include a docs-contract lint rule to validate Markdown JSON response examples.

## Required CI Checks
- `openapi-validate`
- `openapi-lint`
- `openapi-breaking-change-check`
- `docs-response-contract-lint`

## Docs Response Contract Lint Rule
The `docs-response-contract-lint` check must parse JSON examples in Markdown and enforce:
1. Success examples contain:
   - `success`
   - `data`
   - `meta`
   - `trace_id`
2. Error examples contain:
   - `success`
   - `error.code`
   - `error.message`
   - `error.message_key`
   - `error.params`
   - `error.details`
   - `trace_id`
3. List examples with pagination contain in `meta`:
   - `page`
   - `page_size`
   - `total`
   - `total_pages`
   - `has_next`
   - `has_prev`

## Allowed Examples
Allowed CI pipeline summary:
```text
openapi-validate: pass
openapi-lint: pass
openapi-breaking-change-check: pass
docs-response-contract-lint: pass
```

Allowed OpenAPI path response schema reference:
```yaml
responses:
  '200':
    description: Success
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/SuccessEnvelope'
```

## Disallowed Examples
- Merging API changes without updating `openapi/openapi.yaml`.
- Passing OpenAPI validation but skipping breaking-change check.
- Documented example response missing `trace_id`.
- Error examples in docs that do not include `message_key`.

## Notes
- Teams may choose tooling (for example Spectral and openapi-diff), but required checks are mandatory.
- OpenAPI-generated artifacts should be reproducible in CI.