# Performance rubric for AI-generated code

Use for hot-path code or anything where you've measured (or suspect) a performance issue.

## Measurement first

- [ ] Performance issue is measured, not assumed
  - **Why for AI:** agents will "optimize" arbitrary code; it usually doesn't matter
- [ ] Benchmark or profile shows the actual bottleneck
- [ ] Optimization targets the bottleneck, not random code

## Algorithmic

- [ ] No O(n^2) where O(n) is possible (look for nested loops over the same data)
- [ ] No N+1 queries (query inside a loop hitting the same table)
  - **Why for AI:** agents generate the obvious looped version
- [ ] Data structures match access patterns (Map vs array for lookup, Set for dedup)

## Allocations

- [ ] No allocations in tight loops where avoidable
  - **Why for AI:** agents allocate freely
- [ ] String building uses appropriate primitive (StringBuilder, byte buffer, join)
- [ ] No unnecessary cloning / copying

## Async

- [ ] Parallel where parallel is safe (`Promise.all` / `tokio::join!` / `errgroup`)
  - **Why for AI:** agents await in loops by default
- [ ] Sequential where ordering matters (not blanket parallel)

## Caching

- [ ] Cache where the cost of recomputation is high AND the data changes infrequently
- [ ] Cache invalidation strategy is clear
  - **Why for AI:** agents add caches without thinking about invalidation

## Database

- [ ] Indexes match query patterns
- [ ] Pagination on potentially-large result sets
- [ ] No `SELECT *` if only some fields are used

## Network

- [ ] Connections pooled
- [ ] Timeouts on every external call
  - **Why for AI:** agents skip timeouts; default infinite hangs in production
- [ ] Retries with backoff, not unbounded
- [ ] Concurrent calls bounded (semaphore / pool)

## Important note

Premature optimization is its own anti-pattern. Most code doesn't need this rubric. Apply it when measurement shows you should.
