# Tests rubric for AI-generated tests

## Each test

- [ ] Has at least one meaningful assertion
  - **Why for AI:** agents write empty or `toBeDefined`-only tests
  - **Verify:** read each test; if you can delete the assertion and the test still "passes," it's fake

- [ ] Tests behavior, not implementation details
  - **Why for AI:** agents test "method X was called" instead of "given input Y, output is Z"
  - **Verify:** if implementation changes but behavior doesn't, do tests still pass?

- [ ] Doesn't mock the function under test
  - **Why for AI:** common bizarre pattern from agents
  - **Verify:** mock targets vs test name

- [ ] Doesn't share mutable state with other tests
  - **Verify:** tests should pass in any order

## Coverage of cases

- [ ] Happy path
- [ ] At least one edge case (empty input, single element, max size)
- [ ] At least one error case (invalid input, network failure, etc.)
- [ ] Boundary values
  - **Why for AI:** agents test middle-of-the-road inputs and miss boundaries

## Mocks

- [ ] Mocks only the boundary (DB, network, filesystem), not internal logic
- [ ] Mock setup matches realistic responses (real API shapes, real error codes)
  - **Why for AI:** agents fabricate mock responses that don't match reality

## Test organization

- [ ] Tests are co-located with code or in a clear test directory
- [ ] Test names describe behavior: `loads user when id is valid`, not `test1`
- [ ] Setup/teardown is in fixtures, not duplicated across tests

## Mutation testing (optional but powerful)

- [ ] Tests fail when you mutate the implementation (e.g. invert an operator)
  - **Why for AI:** if all tests still pass with a broken implementation, the tests test nothing
  - **Tools:** stryker (JS), mutmut (Python), Pitest (JVM)

## Integration vs unit

- [ ] Unit tests don't hit external services
- [ ] Integration tests hit real services (or representative test doubles)
- [ ] No "integration tests" that are really unit tests with everything mocked
  - **Why for AI:** agents call things "integration" because they cross modules even when they mock everything
