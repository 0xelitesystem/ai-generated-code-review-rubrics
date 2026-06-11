# Go rubric for AI-generated code

## Errors

- [ ] Errors are checked at every callsite (no `_, _ := f()`)
  - **Why for AI:** agents sometimes ignore errors with `_`
  - **Verify:** `errcheck` linter

- [ ] Errors wrapped with context using `fmt.Errorf("...%w", err)`
  - **Why for AI:** agents return raw errors that lose context

- [ ] No panics in library code (panic only in `main` for unrecoverable startup)
  - **Verify:** grep `panic(`

- [ ] `defer` used for cleanup; no unclosed resources
  - **Verify:** look for `os.Open`, `sql.Open`, etc., without paired `defer Close()`

## Concurrency

- [ ] Goroutines have a clear lifecycle (when do they exit?)
  - **Why for AI:** agents spawn goroutines without teardown

- [ ] Channels closed by sender, not receiver
  - **Why for AI:** common Go bug agents replicate

- [ ] No data races (run with `-race` flag in tests)
  - **Verify:** `go test -race ./...`

- [ ] Context propagation: every function that does I/O takes a `context.Context` as first param
  - **Why for AI:** agents skip context propagation in deep calls

## Standards

- [ ] No use of `interface{}` without justification (use `any` if Go 1.18+, but still justify)
  - **Why for AI:** `interface{}` / `any` is the agent's escape hatch

- [ ] No reflection without strong justification
  - **Why for AI:** agents reach for `reflect` instead of redesigning

- [ ] No `time.Sleep` in tests (use proper synchronization)
  - **Why for AI:** agents use sleep to "fix" race conditions

## Tests

- [ ] Tests use `t.Helper()` in test helpers
- [ ] Table-driven tests where appropriate
- [ ] No `t.Skip()` without explanation
- [ ] Mocks built with interfaces in production code, not introduced just for testing
  - **Why for AI:** agents introduce interfaces "for testing" that complicate the design

## Imports

- [ ] Standard library used in preference to third-party where equivalent
  - **Why for AI:** agents reach for `gorilla/mux` when `http.ServeMux` (1.22+) works

- [ ] No new dependencies in `go.mod` (or each justified)
  - **Verify:** `git diff go.mod go.sum`
