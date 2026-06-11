# Contributing a Rubric

## What makes a good rubric

- Each item is a SPECIFIC check, not a vague principle
- Each item explains why this matters specifically for AI-generated code (the "Why for AI" notes)
- Each item has a verification method (linter, grep, command)
- Items are checklist-shaped (PASS/FAIL/N/A), not essays

## Submission

1. Add a new file in `rubrics/`
2. Add row to README.md
3. Open a PR

PR title: `add rubric: name` or `update rubric: name`

## What gets rejected

- Generic checklists (this is for AI-specific stuff)
- Items without "Why for AI" rationale
- Items where verification is "review carefully"
- Rubrics duplicating existing ones with minor tweaks (improve existing instead)

## License

By contributing, you agree your contribution is MIT-licensed.
