# SQL rubric for AI-generated code

## Performance

- [ ] No `SELECT *` in production queries
  - **Why for AI:** agents default to `SELECT *` even when only some columns are used
  - **Verify:** grep `SELECT \*` in non-debug queries

- [ ] Indexes exist for WHERE clauses on non-trivial tables
  - **Verify:** `EXPLAIN` the query; look for table scans

- [ ] No N+1 patterns (a query inside a loop)
  - **Why for AI:** agents generate the obvious "loop and query" instead of one batched query
  - **Verify:** check the calling code for queries inside loops

- [ ] Joins have ON clauses, not implicit WHERE-based joins
- [ ] LIMIT used on queries that could return unbounded results

## Correctness

- [ ] No string concatenation for query building (parameterized queries)
  - **Why for AI:** agents sometimes build SQL with string concat
  - **Verify:** grep for `+` near SQL strings, `f"...{var}..."` SQL

- [ ] Booleans / nullables handled explicitly (`IS NULL` vs `= NULL`)
  - **Why for AI:** agents use `= NULL` which silently returns nothing

- [ ] Date/time handling uses appropriate types (not text comparisons on dates)

- [ ] DISTINCT used intentionally (not as a band-aid for incorrect joins)

## Migrations

- [ ] Migrations are reversible (down migration exists or is consciously skipped)
  - **Why for AI:** agents skip down migrations

- [ ] Migrations are idempotent or guarded (`CREATE TABLE IF NOT EXISTS`)
- [ ] No data migrations mixed with schema migrations
  - **Why for AI:** agents combine because "while we're here"

- [ ] Long-running migrations have timeouts and are tested on representative data sizes

## Security

- [ ] No queries that include user input directly (parameterized only)
- [ ] Permissions appropriate to the application user (not always superuser)
- [ ] No DROP TABLE / TRUNCATE / DELETE without WHERE in committed code
  - **Verify:** grep for these, each needs explicit justification
