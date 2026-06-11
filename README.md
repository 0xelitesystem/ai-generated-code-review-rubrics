# ai-generated-code-review-rubrics

Code review rubrics specifically calibrated for AI-generated code. Different from human-code rubrics because AI fails differently, confident-but-wrong patterns, hallucinated APIs, fake comprehensiveness, than humans do.

## Why this exists

Standard code review checklists assume human authors. Humans typically don't invent library functions; AI does. Humans typically don't confidently fix the wrong root cause; AI does. Humans typically don't write tests with no assertions; AI sometimes does. Reviewers reviewing AI-generated code need calibrated suspicion.

These rubrics adjust for that.

## Rubrics by language

| Rubric | Use for |
|---|---|
| [python.md](./rubrics/python.md) | Python code (any framework) |
| [typescript.md](./rubrics/typescript.md) | TypeScript / React / Node |
| [go.md](./rubrics/go.md) | Go code |
| [rust.md](./rubrics/rust.md) | Rust code |
| [sql.md](./rubrics/sql.md) | SQL queries and migrations |

## Rubrics by concern

| Rubric | Use for |
|---|---|
| [security.md](./rubrics/security.md) | Any code crossing trust boundaries |
| [performance.md](./rubrics/performance.md) | Hot-path code |
| [tests.md](./rubrics/tests.md) | Reviewing AI-generated tests |
| [config.md](./rubrics/config.md) | YAML/TOML/JSON config files |
| [migrations.md](./rubrics/migrations.md) | Database migrations |

## How to use

1. Pick the rubric matching your situation
2. Walk through each item against the diff
3. Mark each as: PASS / FAIL / N/A
4. Anything FAIL is a reason to request changes

A rubric isn't a substitute for understanding; it's a structured way to apply skepticism efficiently.

## Each rubric's structure

```
- Item: specific check
- Why this matters for AI code: how AI fails this differently
- How to verify: specific command, search, or check
```

## Contribute

PRs welcome. New rubrics for stacks not covered, refinements to existing rubrics, additions for tooling-specific patterns.

## License

MIT.

## Related

- [vibe-coding-anti-patterns](https://github.com/0xelitesystem/vibe-coding-anti-patterns) - failure modes these rubrics catch
- [ai-coding-prompt-recipes](https://github.com/0xelitesystem/ai-coding-prompt-recipes) - prompts that prevent some of these issues
- [vibe-coding-test-strategies](https://github.com/0xelitesystem/vibe-coding-test-strategies) - testing approaches
