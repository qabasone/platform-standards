# Error Catalog Governance Standard

## Purpose
Define lifecycle, ownership, and compatibility rules for shared error codes and localization message keys.

## Rules
1. All known errors must be defined in a centralized shared catalog package.
2. Each catalog entry must include:
   - `error_code`
   - `message_key`
   - owning team
   - severity (`info|warning|error|critical`)
3. `error_code` must be uppercase `SNAKE_CASE` and stable.
4. `message_key` must be lowercase dot notation and stable.
5. Every `message_key` must have localized entries for:
   - `en`
   - `ar-EG`
6. Placeholder names must match exactly across locales.
7. Adding a new error key requires both locales in the same pull request.
8. Renaming or deleting existing keys is a breaking change.
9. Deprecated keys must remain available for at least one major release cycle.
10. Catalog changes must identify owning team in PR description.
11. Catalog CI checks must fail on:
   - duplicate `error_code`
   - duplicate `message_key`
   - missing locale entry
   - placeholder mismatch between locales
12. Runtime services must reference catalog keys, not free-text error strings.

## Allowed Examples
Catalog definition example:
```json
{
  "error_code": "WALLET_INSUFFICIENT_BALANCE",
  "message_key": "wallet.insufficient_balance",
  "owner": "@qabasone/finance-platform",
  "severity": "warning"
}
```

Locale entries:
```json
{
  "wallet.insufficient_balance": "Insufficient balance. You need {required} {currency}, but only have {available}."
}
```

```json
{
  "wallet.insufficient_balance": "Rasedak mesh mokafi. Mehtag {required} {currency}, wel motah delwa2ty {available}."
}
```

## Disallowed Examples
- Key exists in `en` but missing in `ar-EG`.
- `error_code` changed from `WALLET_INSUFFICIENT_BALANCE` to `BALANCE_TOO_LOW` without migration plan.
- Placeholder mismatch:
  - `en`: `{required}`
  - `ar-EG`: `{required_amount}`
- Service returning hardcoded text without `message_key`.

## Notes
- Catalog ownership should align with domain ownership (`auth`, `identity`, `finance`, `logistics`, etc.).
- LLM tooling may help draft translations offline, but final catalog entries are human-reviewed and version-controlled.