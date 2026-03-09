# Health Checks Standard

## Purpose
Define mandatory health endpoints so orchestration, load balancers, and monitoring systems can evaluate service status consistently.

## Rules
1. Every backend service must expose:
   - `GET /health/live`
   - `GET /health/ready`
2. `live` check validates process liveness only; it must not depend on external systems.
3. `ready` check validates readiness to serve traffic and must include critical dependencies.
4. Response format must follow API success/error envelopes.
5. Health endpoints must not require authentication for internal platform probes.
6. `ready` must return non-2xx if required dependencies are unavailable.
7. Readiness response should identify dependency states in `data`.
8. Health endpoints must avoid expensive operations (timeouts under 1 second for each dependency probe).

## Allowed Examples
Liveness success (`200`):
```json
{
  "success": true,
  "data": {
    "status": "live"
  },
  "meta": {},
  "trace_id": "trc_health_001"
}
```

Readiness success (`200`):
```json
{
  "success": true,
  "data": {
    "status": "ready",
    "dependencies": {
      "mongo": "up",
      "redis": "up"
    }
  },
  "meta": {},
  "trace_id": "trc_health_002"
}
```

Readiness failure (`503`):
```json
{
  "success": false,
  "error": {
    "code": "COMMON_DEPENDENCY_UNAVAILABLE",
    "message": "Service is not ready",
    "message_key": "common.dependency_unavailable",
    "params": {},
    "details": {
      "mongo": "down"
    }
  },
  "trace_id": "trc_health_003"
}
```

## Disallowed Examples
- `GET /health` as the only health endpoint.
- `ready` endpoint always returning `200` even when database is down.
- returning HTML or plain text from health endpoints.
- requiring bearer token for health probes.

## Notes
- If platform ingress requires a single endpoint, it may map to `/health/ready`; `live` must still exist for internal probes.
- Dependency list in readiness can vary by service, but the contract shape must remain consistent.
