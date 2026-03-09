# Rate Limit Headers Standard

## Purpose
Define consistent rate-limit signaling headers so clients can safely throttle and retry.

## Rules
1. Endpoints protected by rate limiting must return these headers on success and throttled responses:
   - `X-RateLimit-Limit`
   - `X-RateLimit-Remaining`
   - `X-RateLimit-Reset`
2. `X-RateLimit-Reset` must be Unix epoch seconds.
3. When rate limit is exceeded, service must return `429` with standardized error envelope.
4. `Retry-After` header is mandatory on `429` responses.
5. `Retry-After` must be in seconds.
6. Headers must reflect the active policy scope (user, token, IP, tenant, or endpoint).
7. Header names and semantics must be consistent across all services and gateway responses.
8. Rate-limit error codes must be stable and machine-parseable.

## Header Contract
- `X-RateLimit-Limit`: max requests in current window
- `X-RateLimit-Remaining`: remaining requests in current window
- `X-RateLimit-Reset`: epoch seconds when window resets
- `Retry-After`: seconds until next allowed request (`429` only)

## Allowed Examples
Successful response headers:
```http
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 119
X-RateLimit-Reset: 1773106800
```

Throttled response:
```http
HTTP/1.1 429 Too Many Requests
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1773106800
Retry-After: 30
```

Throttled response body:
```json
{
  "success": false,
  "error": {
    "code": "COMMON_RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please retry later.",
    "message_key": "common.rate_limit_exceeded",
    "params": {
      "retry_after_seconds": 30
    },
    "details": {}
  },
  "trace_id": "trc_rate_001"
}
```

## Disallowed Examples
- Returning `429` without `Retry-After`.
- Returning non-numeric `X-RateLimit-Reset` value.
- Returning plain text body for throttled errors.
- Using different header names across services.

## Notes
- Gateway-level and service-level limits may coexist; externally visible headers must represent effective limit policy.
- Retry behavior should use jitter in clients to avoid synchronized retries.