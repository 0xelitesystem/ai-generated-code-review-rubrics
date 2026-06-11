# Security rubric for AI-generated code

Use for any code crossing trust boundaries.

## Input

- [ ] Every input from untrusted source is validated (HTTP body, query params, headers, file contents, environment vars)
  - **Why for AI:** agents trust types; runtime data isn't typed
  - **Verify:** schema validation (zod/pydantic/etc.) at boundaries

- [ ] Path traversal prevented on file operations
  - **Why for AI:** agents accept `req.body.path` and use it in `fs.readFile`
  - **Verify:** any user-influenced filesystem path normalized and validated

- [ ] No SSRF vectors (user-supplied URLs that the server fetches)
  - **Why for AI:** "fetch this URL for me" feature is common

- [ ] Regex inputs validated for ReDoS-prone patterns
  - **Why for AI:** agents accept user regex without bounding

## Auth

- [ ] Authentication required where applicable
- [ ] Authorization checks aren't skipped or assumed
  - **Why for AI:** agents assume "if you're authenticated you're authorized"
- [ ] No IDOR (Insecure Direct Object Reference): user can only access their own resources
  - **Verify:** every `findById` should be `findById(userId, resourceId)` not just `findById(resourceId)`

## Secrets

- [ ] No hardcoded secrets, API keys, or credentials
  - **Verify:** scan with secrets-scanner-bookmarklet or similar
- [ ] Secrets read from environment variables, not from config files committed to repo
- [ ] No secrets in error messages or logs
  - **Why for AI:** agents log full request bodies including auth headers

## Output

- [ ] No reflection of user input into HTML without escaping
  - **Why for AI:** template literals look safe and aren't always
- [ ] No SQL injection (use parameterized queries; see SQL rubric)
- [ ] No command injection (use array form for spawn, not string)
- [ ] Errors returned to user don't leak stack traces or internal paths

## Rate limiting / abuse

- [ ] Auth endpoints rate-limited
- [ ] Expensive endpoints rate-limited
- [ ] Bulk operations have reasonable bounds

## Cryptography

- [ ] No custom crypto algorithms
  - **Why for AI:** agents will write crypto if asked; almost always wrong
- [ ] No deprecated algorithms (MD5, SHA-1 for security purposes, ECB mode)
- [ ] Hardcoded IVs are never acceptable
- [ ] Secrets compared with constant-time comparison (`crypto.timingSafeEqual`)

## Important caveat

This is a starting checklist, not a complete audit. Real security work requires threat modeling, architecture review, and humans who do this professionally. Don't ship security-critical code based on rubric alone.
