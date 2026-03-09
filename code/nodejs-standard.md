# Node.js Backend Standard

## Purpose
Define backend implementation standards for Node.js services.

## Rules
1. Use active LTS Node.js version (minimum major version approved by platform owners).
2. Use `async/await`; promise chains are allowed only when clearer.
3. Blocking sync I/O (`fs.readFileSync`, etc.) is forbidden in request paths.
4. Graceful shutdown is required for SIGTERM/SIGINT.
5. Services must validate environment variables at startup and fail fast on invalid config.
6. Structured logging with `trace_id` is mandatory.
7. Use explicit timeouts for all outbound network/database calls.
8. Shared mutable global state is forbidden in runtime request handling.
9. Errors must follow the platform structured error contract.

## Allowed Examples
- Boot process validates config before starting HTTP listener.
- Request handlers use async functions with timeout-aware clients.
- Shutdown hook closes DB and HTTP server gracefully.

## Disallowed Examples
- `fs.readFileSync` inside request handler.
- Unbounded outbound call with no timeout.
- Global mutable cache used as source of truth with no invalidation policy.

## Notes
- Worker processes may use separate runtime tuning from API services.
- Prefer dependency injection patterns for testability.