# Go Standard

## Purpose
Define coding standards for Go services used for high-concurrency backend workloads.

## Rules
1. `gofmt` formatting is mandatory.
2. `go vet` and tests must pass in CI.
3. Use `context.Context` as first parameter for request-scoped operations.
4. Errors must be returned, wrapped with context, and never silently ignored.
5. `panic` is forbidden in normal control flow.
6. Concurrency must honor cancellation and timeouts.
7. Shared state across goroutines must be synchronized safely.
8. Keep interfaces near consumers, not producers, unless contract is shared across packages.
9. Public package APIs must include package-level documentation.

## Allowed Examples
```go
func (s *Service) Execute(ctx context.Context, cmd Command) error {
    if err := s.repo.Save(ctx, cmd); err != nil {
        return fmt.Errorf("save command: %w", err)
    }
    return nil
}
```

## Disallowed Examples
```go
func DoWork() {
    panic("unexpected")
}
```
Reason: panic in normal flow is forbidden.

```go
func Load() {
    _ = repo.Save(context.Background(), cmd)
}
```
Reason: ignored error.

## Notes
- Use worker pools or bounded concurrency for fan-out jobs.
- Prefer table-driven tests for business logic.