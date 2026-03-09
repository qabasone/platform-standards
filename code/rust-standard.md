# Rust Standard

## Purpose
Define coding standards for Rust services used for performance-sensitive and highly concurrent backend workloads.

## Rules
1. Use stable Rust toolchain unless platform owners approve nightly features.
2. `cargo fmt --check`, `cargo clippy -- -D warnings`, and tests must pass in CI.
3. `unwrap` and `expect` are forbidden in production code paths.
4. Use `Result`-based error propagation with contextual error messages.
5. Shared mutable state must be synchronized safely (`Arc`, `Mutex`, channels, or lock-free primitives).
6. Async code must avoid blocking operations on async executors.
7. Public API crates/modules must document behavior and error conditions.
8. Unsafe code is forbidden unless justified with comments and review from platform owners.

## Allowed Examples
```rust
fn parse_amount(input: &str) -> Result<u64, AppError> {
    input
        .parse::<u64>()
        .map_err(|e| AppError::new("COMMON_PARSE_FAILED", format!("parse amount: {e}")))
}
```

## Disallowed Examples
```rust
let amount = payload.amount.unwrap();
```
Reason: `unwrap` in production path.

```rust
unsafe {
    do_raw_pointer_stuff();
}
```
Reason: unsafe block without explicit exception process.

## Notes
- Use typed domain wrappers to avoid primitive obsession.
- Keep async boundaries explicit and measurable.