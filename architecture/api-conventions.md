# API Conventions Standard

## Purpose
Define a universal HTTP and JSON contract for all platform APIs so clients, services, and automation can integrate consistently.

## Rules
1. APIs must be RESTful over HTTPS and return `application/json; charset=utf-8`.
2. Version must be in URL path: `/v1/...`.
3. Resource paths must use lowercase nouns in `kebab-case`.
4. HTTP method semantics:
   - `GET` read only
   - `POST` create or trigger action
   - `PUT` full replacement
   - `PATCH` partial update
   - `DELETE` remove
5. Success responses must use this envelope:
```json
{
  "success": true,
  "data": {},
  "meta": {},
  "trace_id": ""
}
```
6. Error responses must use this envelope:
```json
{
  "success": false,
  "error": {
    "code": "",
    "message": "",
    "message_key": "",
    "params": {},
    "details": {}
  },
  "trace_id": ""
}
```
7. `trace_id` is mandatory in every response.
8. `meta` is mandatory and may be `{}` when no metadata is needed.
9. Timestamps must be ISO 8601 UTC (`YYYY-MM-DDTHH:MM:SS.sssZ`).
10. Paginated list endpoints must accept:
    - `page` (integer, default `1`)
    - `page_size` (integer, default `25`, maximum `100`)
11. Pagination fields inside `meta` must be:
    - `page`
    - `page_size`
    - `total`
    - `total_pages`
    - `has_next`
    - `has_prev`
12. API responses must not expose internal stack traces, service internals, or database driver errors.
13. Service-to-service error payloads must carry structured fields (`error_code`, `message_key`, `params`, `http_status`, `trace_id`) and must not include user-facing localized strings.
14. User-facing `error.message` must be generated at edge layers (`api-gateway`, BFF, or shared localization layer).
15. Locale resolution order for edge responses:
    - `Accept-Language` request header
    - user profile locale (if authenticated and available)
    - tenant default locale
    - fallback to `en`
16. Supported locale baseline is `en` and `ar-EG`.
17. `developer_message` is an internal diagnostic field and must never be returned in public API responses.
18. Known errors must resolve from a message catalog by `message_key`; do not run per-request free-text translation as primary behavior.

## Allowed Examples
Success list response:
```json
{
  "success": true,
  "data": [
    {
      "id": "usr_01J5Q8XH9",
      "email": "ops@qabas.one",
      "status": "active"
    }
  ],
  "meta": {
    "page": 1,
    "page_size": 25,
    "total": 120,
    "total_pages": 5,
    "has_next": true,
    "has_prev": false
  },
  "trace_id": "trc_8f4cb66f89d7"
}
```

Action response:
```json
{
  "success": true,
  "data": {
    "access_token": "eyJ...",
    "refresh_token": "eyJ..."
  },
  "meta": {},
  "trace_id": "trc_2fe88f3347e1"
}
```

Localized error response (`ar-EG`):
```json
{
  "success": false,
  "error": {
    "code": "WALLET_INSUFFICIENT_BALANCE",
    "message": "Rasedak mesh mokafi. Mehtag {required} {currency}, wel motah delwa2ty {available}.",
    "message_key": "wallet.insufficient_balance",
    "params": {
      "required": 1500,
      "available": 900,
      "currency": "EGP"
    },
    "details": {}
  },
  "trace_id": "trc_58f830a17d22"
}
```

## Disallowed Examples
```json
{
  "ok": true,
  "payload": {}
}
```
Reason: non-standard envelope keys.

```json
{
  "success": true,
  "data": {},
  "traceId": "abc"
}
```
Reason: `meta` missing and key naming inconsistent.

```json
{
  "success": false,
  "error": "invalid token"
}
```
Reason: `error` must be an object with structured fields.

```json
{
  "success": true,
  "data": [],
  "meta": {
    "page": 1,
    "page_size": 25,
    "total": 120
  },
  "trace_id": "trc_123"
}
```
Reason: paginated list response is missing required pagination metadata (`total_pages`, `has_next`, `has_prev`).

```json
{
  "success": false,
  "error": {
    "code": "ORDER_CANNOT_CANCEL_AFTER_SHIPMENT",
    "message": "Order cannot be cancelled after shipment",
    "message_key": "",
    "params": {}
  }
}
```
Reason: missing required `message_key` for catalog-based localization.

```json
{
  "success": false,
  "error": {
    "code": "AUTH_INVALID_TOKEN",
    "message": "Invalid token",
    "developer_message": "JwtVerificationError at AuthMiddleware line 31"
  }
}
```
Reason: `developer_message` cannot be exposed externally.

## Notes
- `api-gateway` may aggregate downstream data but must still return this same envelope.
- Backward-incompatible response changes require version bump and ADR.
- Use companion standards for advanced behavior:
  - `architecture/pagination-standard.md`
  - `architecture/filtering-and-sorting.md`
  - `architecture/idempotency-standard.md`
  - `architecture/rate-limit-headers.md`
  - `architecture/deprecation-policy.md`
