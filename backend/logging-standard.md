# Logging Standard

## Purpose
Define a production-safe, structured logging contract that supports debugging, alerting, and traceability across services.

## Rules
1. Logs must be structured JSON in all environments.
2. Required log fields:
   - `timestamp`
   - `level`
   - `service_name`
   - `app_env`
   - `message`
   - `trace_id`
3. Allowed log levels:
   - `debug`
   - `info`
   - `warn`
   - `error`
4. `trace_id` must be propagated from request context; background jobs generate one at start.
5. PII and secrets must never be logged (passwords, JWT secrets, refresh tokens, full card/account numbers).
6. Errors must include machine-parseable metadata (`error_code`, `operation`, `status_code` when available).
7. Plain text logging (`console.log("...")`) is forbidden in production code paths.
8. Log message text must be concise and action-oriented.
9. If a structured error contract is used, error logs must include `message_key` and `params` where available.
10. `developer_message` may be logged internally for troubleshooting but must not be returned in public API payloads.
11. Edge services should log resolved `locale` for localized error responses.

## Allowed Examples
```json
{
  "timestamp": "2026-03-09T21:02:41.155Z",
  "level": "info",
  "service_name": "api-gateway",
  "app_env": "staging",
  "message": "request completed",
  "trace_id": "trc_8de42c3f91a7",
  "method": "GET",
  "path": "/v1/users",
  "status_code": 200,
  "duration_ms": 32
}
```

```json
{
  "timestamp": "2026-03-09T21:03:09.010Z",
  "level": "error",
  "service_name": "api-auth",
  "app_env": "production",
  "message": "token refresh failed",
  "trace_id": "trc_6f3a87d7ed2f",
  "error_code": "AUTH_REFRESH_INVALID",
  "message_key": "auth.refresh_invalid",
  "params": {},
  "developer_message": "Refresh token hash mismatch",
  "status_code": 401
}
```

## Disallowed Examples
```text
[ERROR] refresh failed for token eyJhbGciOi...
```
Reason: plain text and token leakage.

```json
{
  "level": "fatal",
  "message": "something happened"
}
```
Reason: missing required fields and invalid log level.

```json
{
  "timestamp": "2026-03-09T21:03:09.010Z",
  "level": "info",
  "message": "user login",
  "password": "secret123"
}
```
Reason: credential logging is forbidden.

```json
{
  "success": false,
  "error": {
    "code": "AUTH_INVALID_TOKEN",
    "developer_message": "JwtParseException line 55"
  }
}
```
Reason: `developer_message` belongs in logs only, not API response payload.

## Notes
- Services may add extra contextual fields, but required fields must always be present.
- Centralized log routing and retention policy are managed by platform operations.
