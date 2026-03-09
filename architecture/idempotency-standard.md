# Idempotency Standard

## Purpose
Prevent duplicate side effects for retried or replayed write requests.

## Rules
1. Idempotency is mandatory for `POST` endpoints that create financial, inventory, logistics, or irreversible operational effects.
2. Clients must send `Idempotency-Key` header for idempotent `POST` endpoints.
3. `Idempotency-Key` must be unique per operation intent and between 8 and 128 characters.
4. Key scope must include:
   - HTTP method
   - normalized path
   - authenticated actor or tenant scope
5. Server must persist key, request fingerprint, response body, and status code for replay.
6. Default idempotency record TTL is `24h` unless endpoint-specific policy extends it.
7. Same key with same request fingerprint must return original status code and body.
8. Same key with different request fingerprint must return `409`.
9. Replayed responses should include `Idempotency-Replayed: true`.
10. Idempotency applies to write operations only; it must not be required for `GET`.

## Required Headers
- Request:
  - `Idempotency-Key: <key>`
- Response:
  - `Idempotency-Key: <key>`
  - `Idempotency-Replayed: true|false` (recommended)

## Allowed Examples
Initial request:
```http
POST /v1/payments
Idempotency-Key: 8b9a4abf-8f7d-4b8f-9f84-0d6d8c9d9133
```

First response:
```http
HTTP/1.1 201 Created
Idempotency-Key: 8b9a4abf-8f7d-4b8f-9f84-0d6d8c9d9133
Idempotency-Replayed: false
```

Retry response with same payload:
```http
HTTP/1.1 201 Created
Idempotency-Key: 8b9a4abf-8f7d-4b8f-9f84-0d6d8c9d9133
Idempotency-Replayed: true
```

Conflict response for same key with changed payload:
```json
{
  "success": false,
  "error": {
    "code": "COMMON_IDEMPOTENCY_KEY_REUSED",
    "message": "Request conflicts with a previous idempotent request.",
    "message_key": "common.idempotency_key_reused",
    "params": {},
    "details": {}
  },
  "trace_id": "trc_9aa0001"
}
```

## Disallowed Examples
- Processing the same idempotency key twice and creating duplicate payments.
- Ignoring request fingerprint and returning old response for a different payload.
- Requiring idempotency key for `GET /v1/users`.
- Using idempotency key as a permanent business identifier.

## Notes
- For queued workflows, idempotency must be enforced before enqueue.
- Persisted idempotency records must not include secrets in plain text.