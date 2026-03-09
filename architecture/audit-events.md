# Audit Events Standard

## Purpose
Define a universal audit event schema for security, compliance, operations traceability, and cross-service observability.

## Rules
1. Every auditable action must emit one audit event.
2. Event payload must include all required fields:
   - `actor_id`
   - `actor_type`
   - `action`
   - `target_type`
   - `target_id`
   - `changes`
   - `timestamp`
   - `request_id`
3. `action` must be uppercase `SCREAMING_SNAKE_CASE`.
4. `actor_type` must be one of:
   - `USER`
   - `SERVICE`
   - `SYSTEM`
5. `timestamp` must be ISO 8601 UTC.
6. `changes` must be an object with `before` and `after` snapshots when state changes occur.
7. For failed operations (for example login failure), `changes` may be empty object `{}`.
8. `request_id` must match the API request trace context.

## Canonical Schema
```json
{
  "actor_id": "usr_01J5Q8XH9",
  "actor_type": "USER",
  "action": "USER_CREATED",
  "target_type": "USER",
  "target_id": "usr_01J5Q8XH9",
  "changes": {
    "before": null,
    "after": {
      "email": "new.user@qabas.one",
      "status": "active"
    }
  },
  "timestamp": "2026-03-09T20:15:30.123Z",
  "request_id": "req_b8a3f2e6019e"
}
```

## Allowed Examples
`USER_CREATED`:
```json
{
  "actor_id": "usr_admin_01",
  "actor_type": "USER",
  "action": "USER_CREATED",
  "target_type": "USER",
  "target_id": "usr_01J5Q8XH9",
  "changes": {
    "before": null,
    "after": {
      "email": "ops@qabas.one",
      "verification_status": "verified"
    }
  },
  "timestamp": "2026-03-09T20:15:30.123Z",
  "request_id": "req_72fbc1570d0a"
}
```

`ROLE_ASSIGNED`:
```json
{
  "actor_id": "usr_admin_01",
  "actor_type": "USER",
  "action": "ROLE_ASSIGNED",
  "target_type": "USER_ROLE",
  "target_id": "usr_01J5Q8XH9",
  "changes": {
    "before": {
      "roles": ["viewer"]
    },
    "after": {
      "roles": ["viewer", "warehouse_manager"]
    }
  },
  "timestamp": "2026-03-09T20:16:10.001Z",
  "request_id": "req_536947f21ef2"
}
```

`LOGIN_FAILED`:
```json
{
  "actor_id": "anonymous",
  "actor_type": "USER",
  "action": "LOGIN_FAILED",
  "target_type": "AUTH_SESSION",
  "target_id": "email:ops@qabas.one",
  "changes": {},
  "timestamp": "2026-03-09T20:20:45.918Z",
  "request_id": "req_1f50ed3d4cab"
}
```

## Disallowed Examples
```json
{
  "actor": "usr_1",
  "event": "user_created"
}
```
Reason: wrong field names and invalid action format.

```json
{
  "actor_id": "usr_1",
  "actor_type": "USER",
  "action": "ROLE_ASSIGNED",
  "timestamp": "09-03-2026 10:20"
}
```
Reason: missing required fields and invalid timestamp format.

## Notes
- Audit events are immutable records and must not be edited after emission.
- Sensitive fields (passwords, secrets, raw tokens) must be redacted before event publishing.
