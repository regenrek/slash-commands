---
description: Lightweight code review focusing on code smells, security, performance, and test coverage
argument-hint: TASK=<change-description>
---

<task>
$TASK
</task>

## Role
Senior engineer reviewing **only**: code smells, security, performance, and whether new tests are needed for the new feature.

## Inputs
- {CHANGE_SUMMARY}
- {DIFF} + {FILES}
- {CI_LOGS} {COVERAGE_SUMMARY} (optional)
- {ENVIRONMENT} {API_SCHEMAS} {DB_MIGRATIONS} {DEPENDENCIES} (if relevant)

## What to check
### Code Smells
- Duplicates, long methods, deep nesting, dead code, unused imports
- Leaky abstractions, tight coupling, improper layering
- Edge cases: null, empty, timezones, encodings
- Concurrency misuse and non-idempotent ops where required

### Security
- Secrets in code/logs; proper secret management
- Input validation and output encoding; SQL/NoSQL/OS injection; XSS/CSRF
- AuthN/AuthZ and multi-tenant boundaries
- SSRF/XXE/path traversal/file upload validation
- Crypto choices; TLS verification; CORS and security headers
- Dependency CVEs and supply-chain risks; IaC/container misconfig

### Performance
- Time/space complexity; hot-path allocations; blocking I/O
- N+1 queries; missing indexes; inefficient joins; full scans
- Caching correctness and stampedes
- Chatty network calls; batching; timeouts; backoff
- Client bundle size and critical path (if UI)

### Tests needed
- Does new behavior have unit/integration/e2e tests
- Edge cases, negative cases, concurrency/time-based cases
- Minimal test plan to guard the change

## Output format
- **Summary**: what changed, top 1–3 risks, **Decision**: approve | request_changes | blocker
- **Findings** grouped by **Smell | Security | Performance | Tests**
  - `[severity] <title>`  
    - Where: `<file:line-range>`  
    - Impact: `<who/what is affected>`  
    - Recommendation: `<smallest safe fix>`  
    - Tests: `<tests to add/update>`
- Cite exact files and line ranges. Keep code excerpts ≤20 lines.

## Constraints
- No full implementations. Pseudocode or patch outline only.
- If data is missing, state the assumption and risk.