# Filtering And Sorting Standard

## Purpose
Define a strict query contract for filtering and sorting list endpoints.

## Rules
1. Filtering and sorting are allowed only on endpoint-defined allowlists.
2. Unknown filter fields or operators must return `400`.
3. Unknown sort fields must return `400`.
4. Filtering syntax must use this format:
   - `filter[<field>][<operator>]=<value>`
5. Allowed operators:
   - `eq`
   - `ne`
   - `gt`
   - `gte`
   - `lt`
   - `lte`
   - `in`
   - `contains`
6. Sorting syntax must use comma-separated fields:
   - ascending: `sort=created_at`
   - descending: `sort=-created_at`
7. Multiple sort fields are allowed and evaluated left to right.
8. Endpoints must define default sort for deterministic results.
9. Filtering values must be typed and validated by schema.
10. Raw SQL, regex engines, or free-form query strings in filters are forbidden.

## Allowed Examples
Filtering requests:
```http
GET /v1/users?filter[status][eq]=active
GET /v1/users?filter[branch_id][in]=br_1,br_2
GET /v1/orders?filter[created_at][gte]=2026-01-01T00:00:00Z
```

Sorting requests:
```http
GET /v1/users?sort=created_at
GET /v1/users?sort=-created_at,email
```

Combined request:
```http
GET /v1/users?filter[status][eq]=active&sort=-created_at&page=1&page_size=25
```

## Disallowed Examples
```http
GET /v1/users?filter[status][regex]=^act
```
Reason: unsupported operator.

```http
GET /v1/users?sort=-password_hash
```
Reason: field is not in sortable allowlist.

```http
GET /v1/users?filter[status]=active
```
Reason: missing operator segment.

```http
GET /v1/users?where=status='active'
```
Reason: free-form query syntax is forbidden.

## Notes
- Per-endpoint documentation must list allowed filter and sort fields.
- Sensitive fields must never be filterable or sortable.