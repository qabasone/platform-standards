# Environment Variables Standard

## Purpose
Define a mandatory environment variable contract across backend services for predictable runtime behavior and secure configuration.

## Rules
1. Environment variable names must be uppercase `SNAKE_CASE`.
2. Variables must be validated at startup; service must fail fast on missing required variables.
3. Secrets must come from secure secret management and must not be committed to Git.
4. `.env.example` may contain non-secret example values only.
5. `APP_ENV` valid values are strictly:
   - `development`
   - `staging`
   - `production`
6. `SERVICE_NAME` must match canonical service naming standard.
7. `PORT` must be a numeric TCP port.
8. URI variables must be absolute URIs including protocol.
9. JWT TTL values must use duration strings (`15m`, `24h`, `7d`).
10. Edge services that localize errors (`api-gateway`/BFF) must define locale behavior explicitly.
11. Locale codes must use BCP 47 identifiers (`en`, `ar-EG`).

## Required Variables
| Variable | Required | Type | Valid/Format | Example |
|---|---|---|---|---|
| `APP_ENV` | Yes | string | `development|staging|production` | `staging` |
| `SERVICE_NAME` | Yes | string | service canonical name | `api-auth` |
| `PORT` | Yes | number | `1-65535` | `8080` |
| `MONGO_URI` | Yes (if Mongo used) | string | URI | `mongodb://mongo:27017/qabas_auth` |
| `REDIS_URL` | Yes (if Redis used) | string | URI | `redis://redis:6379/0` |
| `JWT_SECRET` | Yes (auth services) | string | min 32 chars | `a-very-long-secret-value` |
| `JWT_ACCESS_TTL` | Yes (auth services) | string | duration | `15m` |
| `JWT_REFRESH_TTL` | Yes (auth services) | string | duration | `7d` |
| `LOG_LEVEL` | Yes | string | `debug|info|warn|error` | `info` |
| `SUPPORTED_LOCALES` | Yes (edge localization services) | string | comma-separated BCP 47 list | `en,ar-EG` |
| `DEFAULT_LOCALE` | Yes (edge localization services) | string | one of `SUPPORTED_LOCALES` | `en` |
| `LOCALIZATION_FALLBACK_LOCALE` | Yes (edge localization services) | string | one of `SUPPORTED_LOCALES` | `en` |

## Allowed Examples
```env
APP_ENV=production
SERVICE_NAME=api-auth
PORT=8080
MONGO_URI=mongodb://mongo:27017/qabas_auth
REDIS_URL=redis://redis:6379/0
JWT_SECRET=3dcda0be8d2a1f40a8d95e6b2a96f108
JWT_ACCESS_TTL=15m
JWT_REFRESH_TTL=7d
LOG_LEVEL=info
SUPPORTED_LOCALES=en,ar-EG
DEFAULT_LOCALE=en
LOCALIZATION_FALLBACK_LOCALE=en
```

## Disallowed Examples
```env
app_env=prod
```
Reason: invalid casing and invalid value.

```env
JWT_ACCESS_TTL=900
```
Reason: duration format must include unit.

```env
SERVICE_NAME=AuthService
```
Reason: must match canonical kebab-case service name.

```env
SUPPORTED_LOCALES=en,arabic
```
Reason: locale codes must be valid BCP 47 identifiers (`ar-EG`, not `arabic`).

## Notes
- If a service does not use Mongo or Redis, unused variables should be omitted and documented in service README.
- Edge services without localization responsibilities may omit locale variables.
- Runtime config schema should be versioned with code changes.