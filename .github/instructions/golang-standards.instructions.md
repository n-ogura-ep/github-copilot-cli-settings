---
applyTo: "**/*.go"
---

# Golang Standards

## Style

- Follow Effective Go and standard Go conventions.
- Format all Go source files with `gofmt`; use `goimports` when available.
- Use short, descriptive names; avoid stuttering in package and exported identifiers.
- Keep packages focused and prefer small interfaces defined by the consumer.
- Add Go doc comments to exported packages, types, functions, methods, and constants.

## Error Handling

- Handle every non-ignored error explicitly; do not discard errors without a clear reason.
- Add useful context when returning errors with `fmt.Errorf("...: %w", err)`.
- Use `errors.Is` and `errors.As` for wrapped error inspection.
- Avoid `panic` for normal error handling; reserve it for unrecoverable programmer errors.
- Validate inputs at public boundaries and return clear, actionable errors.

## Context and Concurrency

- Pass `context.Context` as the first parameter; do not store it in structs.
- Honor cancellation and deadlines in blocking or long-running operations.
- Avoid goroutine leaks; make goroutine ownership and shutdown behavior explicit.
- Protect shared mutable state and run concurrency-sensitive tests with the race detector.

## Testing

- Put tests beside the code in `*_test.go` files and name test functions `TestXxx`.
- Prefer table-driven tests with descriptive subtests using `t.Run`.
- Cover the happy path and edge cases, including empty input, missing data, invalid values, and errors.
- Use `t.Helper()` in test helpers and avoid time-dependent or flaky assertions.
- Run `go test ./...` and, when concurrency is involved, `go test -race ./...`.

## Dependencies and Quality

- Prefer the standard library unless an external dependency provides clear value.
- Keep `go.mod` and `go.sum` tidy with `go mod tidy`.
- Run `go vet ./...`; follow the repository's configured linter, such as `golangci-lint`, when present.
- Do not log secrets, credentials, tokens, or sensitive payloads.
