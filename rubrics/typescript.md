# TypeScript rubric for AI-generated code

## Type system

- [ ] No `any` without explicit justification
  - **Why for AI:** `any` is the agent's escape hatch for hard types
  - **Verify:** grep `: any`, `as any`

- [ ] No `as` casts that bypass validation at trust boundaries
  - **Why for AI:** `JSON.parse(x) as User` is a runtime time bomb
  - **Verify:** grep `as ` and check each, runtime data needs runtime validation (zod/valibot)

- [ ] No `// @ts-ignore` or `// @ts-expect-error` without explanation
  - **Verify:** grep these directives

- [ ] No `Function` or `object` types
  - **Why for AI:** agents use these to avoid specifying exact shapes
  - **Verify:** grep `: Function`, `: object`

- [ ] No `!` non-null assertions on data from external sources
  - **Why for AI:** "I know this isn't null" famous last words from an agent

## Imports and dependencies

- [ ] No new dependencies in `package.json` (or each is justified)
  - **Why for AI:** agents reach for lodash/moment/axios when native works
  - **Verify:** `git diff package.json package-lock.json`

- [ ] No imports from packages not in `package.json`
  - **Why for AI:** hallucinated import paths
  - **Verify:** TypeScript will catch most; lockfile diff catches the rest

- [ ] No deep imports into library internals (`lib/some-internal/private`)
  - **Why for AI:** agent guesses internal paths

## React-specific (if applicable)

- [ ] Hooks called unconditionally, at the top level
  - **Why for AI:** agents sometimes wrap hooks in conditions
  - **Verify:** ESLint with `react-hooks/rules-of-hooks`

- [ ] `useEffect` deps are exhaustive
  - **Verify:** ESLint with `react-hooks/exhaustive-deps`

- [ ] No `useEffect` to derive state from props (use `useMemo` or compute inline)
  - **Why for AI:** agents over-use useEffect for things that don't need it

- [ ] Server components vs client components correctly marked (Next.js App Router)
  - **Why for AI:** agents inconsistent about `'use client'` boundary

## Async

- [ ] No floating promises
  - **Verify:** ESLint with `@typescript-eslint/no-floating-promises`

- [ ] `await` inside loops only when sequential is intentional (otherwise `Promise.all`)
  - **Why for AI:** agents `await` in loops, creating sequential bottlenecks

- [ ] Errors handled at boundaries, not wrapped at every layer
  - **Verify:** count `try/catch` per file; should be few, at well-defined boundaries

## Tests

- [ ] Each test has at least one assertion
- [ ] No `expect(value).toBeDefined()` as the only assertion
  - **Why for AI:** agents write `toBeDefined` as a "I tested it" placeholder
- [ ] Mocks don't replace the function under test
- [ ] No tests that pass when the implementation is empty (mutation-test mentally)
