# Rust rubric for AI-generated code

## Ownership and borrowing

- [ ] No unnecessary `.clone()` to dodge borrow-checker
  - **Why for AI:** agents clone aggressively to make code compile rather than restructure
  - **Verify:** review each `.clone()`; is it actually needed?

- [ ] No `Rc<RefCell<T>>` without justification
  - **Why for AI:** agents reach for `Rc<RefCell<T>>` to avoid lifetime work

- [ ] No `unsafe` without justification
  - **Why for AI:** rare from agents but suspicious when present
  - **Verify:** grep `unsafe `; each one needs a comment

- [ ] No `unwrap()` or `expect()` in non-prototype code
  - **Why for AI:** agents `unwrap` to avoid error handling
  - **Verify:** grep `.unwrap()`, `.expect(`

## Errors

- [ ] Custom error types or `anyhow::Error` (consistent across codebase)
  - **Why for AI:** agents mix `Box<dyn Error>`, `anyhow`, and custom enums inconsistently

- [ ] `?` operator used for error propagation (no manual match-and-return)
- [ ] Errors enriched with context (`anyhow::Context`, or `thiserror` `#[from]`)

## Async

- [ ] Runtime is consistent (tokio OR async-std, not both)
  - **Why for AI:** agents may mix runtimes

- [ ] No `block_on` inside async functions
  - **Why for AI:** agents reach for `block_on` to avoid restructuring

- [ ] `async fn` returns implement `Send` where they need to (for spawn)

## Standards

- [ ] No `#[allow(...)]` without justification
  - **Why for AI:** agents silence warnings instead of addressing them

- [ ] `clippy::all` and `clippy::pedantic` clean (or each lint disabled has reason)
  - **Verify:** `cargo clippy --all-targets`

- [ ] No `mod` files with cross-cutting concerns dumped together

## Tests

- [ ] `#[test]` functions test behavior, not implementation
- [ ] Async tests use proper test attribute (`#[tokio::test]` etc.)
- [ ] No `#[ignore]` without explanation

## Dependencies

- [ ] No new crates in `Cargo.toml` (or each justified)
  - **Verify:** `git diff Cargo.toml Cargo.lock`
- [ ] Feature flags used to avoid pulling unused functionality
