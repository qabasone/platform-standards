# Error Format Standard

## Purpose
Define a strict multilingual error contract that separates:
- internal technical errors produced by services
- localized user-facing errors returned by edge APIs

## Rules
1. Microservices must emit an internal structured error payload with these required fields:
```json
{
  "error_code": "",
  "message_key": "",
  "params": {},
  "http_status": 0,
  "trace_id": ""
}
```
2. `error_code` must be uppercase `SNAKE_CASE` and stable over time (example: `ORDER_CANNOT_CANCEL_AFTER_SHIPMENT`).
3. `developer_message` is optional and allowed only for logs/tracing; it must not be sent to end users.
4. Microservices must not hardcode user-facing message strings in error responses.
5. `message_key` must be stable, lowercase, and dot-separated (example: `order.cannot_cancel_after_shipment`).
6. `params` must be an object containing only named placeholders used by `message_key`.
7. API Gateway/BFF/shared localization layer must resolve `message_key` to localized user text.
8. Supported locale baseline is:
   - `en`
   - `ar-EG`
9. Locale fallback must be deterministic: requested locale -> tenant/user default -> `en`.
10. Public API error payload must follow the global error envelope:
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
11. `trace_id` is mandatory in both internal and external error payloads.
12. HTTP status code must align with error class:
   - `400` validation or malformed request
   - `401` authentication failure
   - `403` authorization failure
   - `404` resource not found
   - `409` conflict
   - `429` rate limited
   - `500` unexpected server error
13. Stack traces, SQL fragments, service internals, and secrets must never appear in user-facing `message` or `details`.
14. Known errors must be resolved from a versioned message catalog; LLM-generated runtime text is not the primary source for known keys.

## Allowed Examples
Internal error emitted by a microservice:
```json
{
  "error_code": "ORDER_CANNOT_CANCEL_AFTER_SHIPMENT",
  "message_key": "order.cannot_cancel_after_shipment",
  "params": {},
  "http_status": 409,
  "trace_id": "trc_9f2c1d",
  "developer_message": "Order status is SHIPPED; cancellation is blocked by policy"
}
```

Public response after edge localization (`en`):
```json
{
  "success": false,
  "error": {
    "code": "ORDER_CANNOT_CANCEL_AFTER_SHIPMENT",
    "message": "This order can't be cancelled after shipment.",
    "message_key": "order.cannot_cancel_after_shipment",
    "params": {},
    "details": {}
  },
  "trace_id": "trc_9f2c1d"
}
```

Public response after edge localization (`ar-EG`):
```json
{
  "success": false,
  "error": {
    "code": "ORDER_CANNOT_CANCEL_AFTER_SHIPMENT",
    "message": "Minfa3sh nelghi el-order ba3d ma etshahan.",
    "message_key": "order.cannot_cancel_after_shipment",
    "params": {},
    "details": {}
  },
  "trace_id": "trc_9f2c1d"
}
```

## Disallowed Examples
```json
{
  "error_code": "ORDER_CANNOT_CANCEL_AFTER_SHIPMENT",
  "message": "Order cannot be cancelled after shipment"
}
```
Reason: service returned hardcoded user-facing message instead of `message_key`.

```json
{
  "success": false,
  "error": {
    "code": "AUTH_INVALID_CREDENTIALS",
    "message": "Invalid token",
    "developer_message": "JwtSignatureVerificationException at AuthGuard line 88"
  }
}
```
Reason: `developer_message` must never be exposed in external response.

```json
{
  "error_code": "INSUFFICIENT_BALANCE",
  "message_key": "wallet.balance_low_dynamic_ai_only",
  "params": {}
}
```
Reason: known error keys must map to static catalog entries, not AI-only runtime messages.

## Notes
- Error codes and message keys are stable contract items and must be versioned carefully.
- Keep `developer_message` in English technical wording for logs and support only.
- `ar-EG` user messages must be friendly, clear, and professional Egyptian Arabic.

## Message Catalog Standard
Catalogs must be versioned files keyed by `message_key`.

`en.json` example:
```json
{
  "wallet.insufficient_balance": "Insufficient balance. You need {required} {currency}, but only have {available}."
}
```

`ar-EG.json` example (ASCII transliteration for documentation safety):
```json
{
  "wallet.insufficient_balance": "Rasedak mesh mokafi. Mehtag {required} {currency}, wel motah delwa2ty {available}."
}
```

## Tone Guide For ar-EG
1. Tone must be friendly, simple, and direct.
2. Avoid rigid formal phrasing for routine UX errors.
3. Keep messages short and actionable.
4. Do not expose technical internals to users.

Allowed phrasing style (transliterated):
- `Hasal moshkela w ehna ben3aleg el-talab.`
- `El-kameya di mesh saheha.`
- `Mesh masmouh leek ta3mel el-egraa da.`
- `El-galsa entahat, saggel dokhool tani.`

Disallowed phrasing style (for user-facing UI):
- `Haddatha khata athna mo3alagat el-talab.` (too formal)
- `Ta3athara tanfeedh el-amr.` (too formal)
