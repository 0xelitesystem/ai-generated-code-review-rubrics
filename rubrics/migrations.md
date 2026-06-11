# Database migrations rubric for AI-generated migrations

## Reversibility

- [ ] Down migration exists OR there's a documented reason it's irreversible
  - **Why for AI:** agents skip down migrations
- [ ] Down migration tested (run up, then down, confirm clean state)

## Safety

- [ ] No `DROP TABLE` / `DROP COLUMN` without explicit deprecation period
- [ ] No `ALTER COLUMN` that loses data (e.g. shrinking a varchar)
  - **Why for AI:** agents don't think about existing data

- [ ] Long-running operations on large tables are batched
  - **Why for AI:** "ADD COLUMN with NOT NULL" on a 100M row table will lock the table
  - Use: add nullable, backfill in batches, then alter to NOT NULL

- [ ] Indexes added with CONCURRENTLY (Postgres) / online (MySQL) where possible

## Data migrations

- [ ] Separate from schema migrations
  - **Why for AI:** agents combine "while we're here"
- [ ] Idempotent (safe to re-run)
- [ ] Bounded (LIMIT or batched, not unbounded UPDATE)

## Naming and order

- [ ] Migration filename has timestamp or sequence number
- [ ] Migrations apply in deterministic order
- [ ] No two PRs creating migrations with the same number

## Rollout

- [ ] Migration tested on a copy of production data (or representative size)
  - **Why for AI:** dev databases are tiny; production isn't
- [ ] Plan exists for migration time and DB load
- [ ] Application code can run with both old and new schemas during deployment
  - **Why for AI:** agents write migrations as if they're atomic with deploy
