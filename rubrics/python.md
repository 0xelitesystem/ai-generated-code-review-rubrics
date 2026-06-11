# Python rubric for AI-generated code

## Imports and dependencies

- [ ] No new top-level dependencies in `pyproject.toml` / `requirements.txt` (or any new ones are explicitly justified)
  - **Why for AI:** agents reach for `requests` / `pandas` / `pydantic` reflexively even when stdlib works
  - **Verify:** `git diff pyproject.toml requirements.txt`

- [ ] No imports from libraries not already in dependencies
  - **Why for AI:** hallucinated imports compile-fail at runtime, not import time
  - **Verify:** run the file or `python -c "import yourmodule"`

- [ ] No `from x import *`
  - **Why for AI:** wildcard imports often hallucinated; agent doesn't know what's exported
  - **Verify:** grep `^from .* import \*`

## Type hints

- [ ] Type hints match runtime behavior (not aspirational)
  - **Why for AI:** agents annotate `-> User` and return `None` for some paths
  - **Verify:** `mypy --strict` (or `pyright`) on the changed files

- [ ] No `# type: ignore` without explanation
  - **Why for AI:** agents add `# type: ignore` to silence errors instead of fixing
  - **Verify:** grep `# type: ignore`; each one needs a reason

- [ ] `Any` is used sparingly; never as a return type without justification
  - **Why for AI:** `Any` is the agent's escape hatch when types are hard
  - **Verify:** grep `: Any`, `-> Any`

## Error handling

- [ ] Exceptions caught are specific, not bare `except:` or `except Exception`
  - **Why for AI:** agents catch broadly to make tests pass
  - **Verify:** grep `except:` and `except Exception:`, each needs a reason

- [ ] No `pass` inside `except` blocks (silent failure)
  - **Verify:** grep for `except.*:\s*\n\s*pass`

- [ ] Errors are either handled meaningfully or propagated, not "logged and continued"
  - **Why for AI:** agents log-and-continue as a "robustness" pattern that hides bugs

## Tests

- [ ] Test functions have at least one `assert`
  - **Why for AI:** agents write empty tests that pass tautologically
  - **Verify:** grep test functions for `assert` keyword

- [ ] Tests use parametrize for multiple cases instead of one big test with internal loops
  - **Verify:** look for `@pytest.mark.parametrize` for multi-case tests

- [ ] Mocks don't replace the function under test
  - **Why for AI:** agents will mock the function they're supposedly testing
  - **Verify:** read mock targets vs the test name

- [ ] Property-based tests for pure functions where applicable
  - **Why for AI:** example-based tests miss edge cases that agents also miss

## Standards

- [ ] No `print()` statements in non-CLI code
  - **Why for AI:** agents add prints for debugging and forget to remove
  - **Verify:** grep `^\s*print(`

- [ ] No commented-out code
  - **Why for AI:** agents leave old code commented during edits
  - **Verify:** grep `^\s*#.*[a-z]\(`

- [ ] No `TODO` without context (date, person, what)
  - **Why for AI:** agents drop TODOs as cover for incomplete work
