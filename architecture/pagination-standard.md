# Pagination Standard

## Purpose
Define consistent pagination behavior for all list endpoints, including page-based and cursor-based pagination.

## Rules
1. Every list endpoint must choose exactly one pagination mode:
   - page-based pagination
   - cursor-based pagination
2. Page-based pagination is required for low to medium cardinality lists where total counts are cheap to compute.
3. Cursor-based pagination is required for high-volume or append-heavy lists where offset/page scans are expensive.
4. An endpoint must not accept both page and cursor parameters in the same request.
5. Default page size/limit is `25`; maximum is `100`.
6. Requests exceeding maximum page size/limit must return `400` with standardized error envelope.
7. Sorting must be deterministic for both modes.
8. Cursor tokens must be opaque and tamper-resistant (signed or encrypted).
9. Pagination metadata must be returned in `meta` only.
10. Pagination parameters and response fields must be documented in OpenAPI.

## Page-Based Contract
Request query params:
- `page` (integer, default `1`, minimum `1`)
- `page_size` (integer, default `25`, minimum `1`, maximum `100`)

Response `meta` fields:
- `page`
- `page_size`
- `total`
- `total_pages`
- `has_next`
- `has_prev`

## Cursor-Based Contract
Request query params:
- `cursor` (string, optional)
- `limit` (integer, default `25`, minimum `1`, maximum `100`)

Response `meta` fields:
- `limit`
- `next_cursor` (string or `null`)
- `prev_cursor` (string or `null`)
- `has_next`
- `has_prev`

## Allowed Examples
Page-based request:
```http
GET /v1/users?page=2&page_size=25
```

Page-based response:
```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 2,
    "page_size": 25,
    "total": 312,
    "total_pages": 13,
    "has_next": true,
    "has_prev": true
  },
  "trace_id": "trc_aa11bb22"
}
```

Cursor-based request:
```http
GET /v1/audit-events?cursor=eyJpZCI6IjEwMDAifQ==&limit=50
```

Cursor-based response:
```json
{
  "success": true,
  "data": [],
  "meta": {
    "limit": 50,
    "next_cursor": "eyJpZCI6IjEwNTAifQ==",
    "prev_cursor": "eyJpZCI6Ijk1MCJ9",
    "has_next": true,
    "has_prev": true
  },
  "trace_id": "trc_cc33dd44"
}
```

## Disallowed Examples
```http
GET /v1/users?page=1&page_size=25&cursor=abc
```
Reason: mixed pagination modes.

```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 1,
    "page_size": 25
  },
  "trace_id": "trc_1"
}
```
Reason: required page-based pagination metadata is incomplete.

```http
GET /v1/audit-events?limit=5000
```
Reason: exceeds maximum limit.

## Notes
- Use cursor mode for event streams, audit feeds, and high-volume timelines.
- Use page mode for admin grids that need total counts.