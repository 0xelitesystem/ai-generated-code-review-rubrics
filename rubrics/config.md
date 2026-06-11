# Config files rubric for AI-generated configs

## YAML / TOML / JSON config

- [ ] Validates against the tool's schema
  - **Why for AI:** agents pattern-match config shapes; specific keys often wrong
  - **Verify:** run the tool with `--validate` / `--lint` / dry-run

- [ ] Versions specified explicitly (don't rely on `latest` defaults)
  - **Why for AI:** agents may use deprecated keys for old versions

- [ ] No secrets committed (use env vars, secret managers, etc.)
- [ ] Comments explain non-obvious choices

## Kubernetes / Docker

- [ ] Resource requests AND limits set
- [ ] Health checks (liveness, readiness) defined
- [ ] No `:latest` tags on images
- [ ] Secrets via Secret resources or external secret manager, not env literals

## Terraform / IaC

- [ ] State backend configured (not local)
- [ ] No hardcoded resource IDs / names that should be variables
- [ ] Variables have descriptions and types
- [ ] No `count = 0` / commented resources without removal plan

## CI / CD

- [ ] Build step caches dependencies
- [ ] Test step actually runs tests (not just compiles)
- [ ] Secrets use the platform's secrets mechanism (not env literals in YAML)
- [ ] Deploy step has a rollback path

## Linter / formatter configs

- [ ] Consistent across the team (everyone agrees on the rules)
- [ ] Auto-format on commit (pre-commit hook or CI check)
- [ ] No conflicting rules between linters (eslint vs prettier conflicts, etc.)
