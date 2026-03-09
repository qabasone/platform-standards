# Data Access Standard (Postgres, MongoDB, Redis)

## Purpose
Define consistent data-access rules across relational, document, and cache layers.

## Rules
1. Choose data store by workload, not team preference:
   - PostgreSQL: transactional/relational source-of-truth data
   - MongoDB: flexible document models and event-like records
   - Redis: cache, rate-limit counters, locks, ephemeral session/state
2. Data ownership is per service; cross-service direct DB access is forbidden.
3. All database operations must use explicit timeouts.
4. Schema changes must use versioned migrations.
5. Queries must use indexes for hot paths.
6. `SELECT *` is forbidden in production queries.
7. Redis keys must be namespaced: `<service>:<domain>:<key>`.
8. Redis entries must define TTL unless explicitly persistent.
9. Mongo collections must define schema validation strategy and index policy.
10. Sensitive data must be encrypted at rest and masked in logs.
11. Connection pools must be configured and bounded.

## PostgreSQL Rules
- Use transactions for multi-step consistency.
- Define unique constraints for natural business invariants.
- Use prepared statements/parameterized queries only.

## MongoDB Rules
- Keep documents aligned with access patterns.
- Avoid unbounded document growth.
- Use compound indexes for high-frequency queries.

## Redis Rules
- Cache keys must include version when schema changes are possible.
- Use lock keys carefully with expiration to avoid deadlocks.
- Never store long-lived source-of-truth records in Redis.

## Allowed Examples
- Postgres: `orders` table with transaction-safe order + ledger write.
- MongoDB: `audit_events` collection with TTL/index strategy when appropriate.
- Redis key: `api-gateway:rate-limit:user:usr_123` with TTL.

## Disallowed Examples
- `api-auth` reading `api-identity` Postgres tables directly.
- Querying Postgres hot path without index.
- Redis key without namespace (`user_123`).
- Using MongoDB as shared cross-service global database.

## Notes
- Data-model tradeoffs must be captured in ADRs for major changes.
- Backup and restore strategy is mandatory for each persistent data store.