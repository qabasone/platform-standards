# Shared Package Naming Standard

## Purpose
Define naming rules for shared packages so dependencies are discoverable and intent is clear across repositories.

## Rules
1. Shared packages must use npm scope `@qabasone/`.
2. Package names must be lowercase `kebab-case`.
3. Package names must include category prefix:
   - `types-`
   - `schemas-`
   - `config-`
   - `utils-`
   - `i18n-`
   - `errors-`
4. Package names must represent one reusable concern.
5. Versioning must follow SemVer and be independent per package.
6. Package directory name and `package.json` name must match.

## Allowed Examples
- `@qabasone/types-auth`
- `@qabasone/schemas-identity`
- `@qabasone/config-env`
- `@qabasone/utils-date`
- `@qabasone/i18n-errors`
- `@qabasone/errors-contract`

## Disallowed Examples
- `@qabasone/AuthTypes` (invalid casing)
- `@qabasone/shared` (too generic)
- `@qabasone/service-auth` (service implementation naming in shared package)
- `qabasone/types-auth` (missing scope syntax)
- `@qabasone/translations` (missing required category prefix)

## Notes
- Names should avoid coupling to one repository unless package is intentionally private to that bounded context.
- Breaking changes in shared packages require release notes and dependent service impact summary.
