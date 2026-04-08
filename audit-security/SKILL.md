---
name: audit-security
description: Audit code for security vulnerabilities such as broken access control, missing authorization, injection risks, secret exposure, unsafe file or process handling, SSRF, open redirects, cross-tenant data leaks, and insecure defaults. Use when reviewing routes, controllers, services, jobs, API handlers, admin tools, background workflows, infrastructure code, or any change that could affect trust boundaries, data exposure, or attacker-controlled input.
---

# Audit Security

Audit trust boundaries before changing behavior. Find where untrusted input, privilege, secrets, or sensitive data are handled unsafely, then apply the smallest safe fix.

## Follow this workflow

1. Map the trust boundaries and privileged operations in scope.
2. Trace attacker-controlled input from entrypoint to sink.
3. Classify each risky path as access control, validation, injection, data exposure, secret handling, file or process safety, network trust, tenant isolation, or insecure default.
4. Flag vulnerabilities, exploit paths, and weak assumptions that could become vulnerabilities.
5. Prioritize findings as `P1` through `P4`.
6. Auto-fix only local, low-risk issues. Leave broader security changes as findings with a concrete fix plan.

## Map the trust boundaries first

Do not audit a single function in isolation when the real risk depends on who can call it, what data reaches it, and what authority it has.

Inspect the relevant equivalents of:

- public routes, API handlers, webhooks, and RPC endpoints
- controllers, actions, and UI-triggered mutations
- admin panels, internal tools, and operator-only paths
- service-layer authorization and business-rule enforcement
- background jobs, schedulers, and message consumers
- database reads and writes involving sensitive or cross-tenant data
- filesystem, shell, template, and serialization boundaries
- outbound HTTP, redirect, callback, and proxy behavior
- secrets, tokens, credentials, and signing keys
- logs, telemetry, caches, exports, and debugging surfaces

Security findings are weak until the relevant caller, data flow, and authority boundary are understood.

## Search for security signals

Start with the language-appropriate equivalents of:

- `auth`
- `authorize`
- `permission`
- `role`
- `admin`
- `policy`
- `guard`
- `tenant`
- `account_id`
- `organization_id`
- `user_id`
- `input`
- `params`
- `query`
- `body`
- `headers`
- `cookie`
- `redirect`
- `fetch`
- `http`
- `request`
- `sql`
- `exec`
- `spawn`
- `system`
- `eval`
- `template`
- `serialize`
- `deserialize`
- `secret`
- `token`
- `password`
- `api_key`

Also inspect code that:

- builds queries, commands, paths, or URLs from input
- branches on roles or permissions in controllers but not deeper layers
- reads records without scoping them to the current actor or tenant
- logs request bodies, headers, tokens, or sensitive payloads
- exposes debug, impersonation, import, export, or backfill functionality
- trusts webhook, callback, proxy, or internal-network requests by origin alone

## Detect these security vulnerabilities

### Broken access control

Flag code that lets callers perform actions or read data without the required permission checks.

Common signals:

- mutation or admin routes with authentication but no authorization
- ownership checks performed in the UI but missing in the API or service layer
- controller-level role checks with deeper reuse paths that bypass them
- privileged actions exposed through background jobs, scripts, or internal endpoints

Prefer authorization at the boundary that owns the business rule, not only at the presentation layer.

### Missing or weak validation

Flag paths where untrusted input reaches sensitive logic without sufficient shape, type, range, or allowlist validation.

Common signals:

- request bodies passed directly into models, ORM updates, or service methods
- dynamic field names, sort keys, filters, or operators accepted unchecked
- IDs, file names, URLs, or MIME types trusted without normalization or validation
- partial validation that checks presence but not allowed values or size limits

Prefer explicit allowlists and typed validation over ad hoc sanitization.

### Injection risk

Flag code where attacker-controlled input can alter the meaning of a query, command, template, path, or interpreter boundary.

Common signals:

- string-built SQL, shell commands, regexes, templates, or HTML
- raw query fragments, order clauses, or column names assembled from input
- command execution with interpolated arguments
- markdown, HTML, or template rendering of untrusted content without the expected escaping path

Prefer parameterization, escaping at the correct sink, and fixed command or query structure.

### Secret exposure

Flag secrets, credentials, or security tokens that can leak through code, config, logs, or client-visible responses.

Common signals:

- hard-coded secrets in source, tests, fixtures, or config defaults
- access tokens, API keys, or passwords written to logs, telemetry, or exceptions
- secret-bearing objects serialized into responses, caches, or job payloads
- debugging endpoints or admin pages showing raw credentials or signed URLs

Prefer secret indirection, redaction, and least-exposure handling.

### Unsafe file or process handling

Flag code that lets input control filesystem or process behavior beyond the intended sandbox.

Common signals:

- paths built from user input without normalization and root checks
- upload, archive, or extraction code that can overwrite arbitrary files
- shell execution using untrusted strings
- unsafe deserialization, dynamic code loading, or parser behavior on attacker-controlled files

Prefer fixed directories, path normalization, argument arrays, and safe parsers.

### SSRF, open redirect, and network trust failures

Flag code that trusts attacker-influenced URLs, hosts, redirects, or network location too much.

Common signals:

- server-side fetches to arbitrary URLs or partially-controlled hosts
- redirect targets taken from query params or headers without strict allowlists
- webhook or internal-only access granted based on IP, host header, or origin assumptions alone
- proxy or callback features that forward credentials or internal metadata

Prefer destination allowlists, canonical host checks, and explicit trust boundaries.

### Cross-tenant or cross-user data leaks

Flag code that can read, write, cache, or expose one tenant's or user's data in another tenant's context.

Common signals:

- record lookups by bare ID without tenant or ownership scope
- caches keyed without tenant, actor, or permission context
- exports, reports, or admin search paths returning mixed-tenant results
- background jobs replaying work with stale or missing tenant context

Require tenant and ownership scoping at every sensitive data boundary, not only in the UI.

### Insecure defaults and dangerous fallback behavior

Flag code whose default state is more permissive than intended or whose failure mode drops protections.

Common signals:

- feature flags, config defaults, or environment fallbacks that disable auth, rate limits, or verification
- optional policy checks that silently skip when context is missing
- exceptions in auth or policy code treated as allow rather than deny
- debug modes, dev credentials, or broad CORS settings that can reach production paths

Prefer deny-by-default and explicit opt-in for privileged behavior.

## Guard against false confidence

Security audits are easy to under-report when trust assumptions are implicit.

Be careful around:

- framework-provided authz, escaping, CSRF, or ORM parameterization that may already mitigate the issue
- internal-only routes that might still be reachable through jobs, proxies, or admin tooling
- generated queries or SDK calls where unsafe parts are hidden one layer down
- caches, reports, exports, and search indexes that persist data after the original read path
- test fixtures or local dev helpers that look dangerous but are not shippable
- external security controls such as WAFs, service mesh policy, or secret managers that cannot be verified locally

If safety depends on external controls or framework guarantees, report the finding with the missing proof instead of asserting certainty you do not have.

## Classify findings

### `P1`

Use for vulnerabilities that can plausibly lead to unauthorized data access, privilege escalation, remote code execution, secret compromise, cross-tenant exposure, payment or billing abuse, or production-impacting exploitation in normal reachable paths.

### `P2`

Use for serious security weaknesses in active paths that are not obviously exploitable immediately but materially reduce the system's safety margin, especially missing authorization, unsafe trust of input, or sensitive data handling mistakes.

### `P3`

Use for defense-in-depth gaps, weak defaults, partial validation, over-broad internal trust, or sensitive logging that raises breach risk without a clear direct exploit shown yet.

### `P4`

Use for low-risk cleanup such as overexposed debug metadata in non-production paths, stale permissive config, or security hardening work with limited immediate exploitability.

## Auto-fix only when safe

Auto-fix local issues only when the intended secure behavior is unambiguous and the blast radius is contained.

Safe examples:

- redact a clearly sensitive value from a local log statement
- replace string interpolation with an existing parameterized query helper already used nearby
- normalize a file path and enforce an existing root-directory guard in one module
- tighten a local redirect helper to an existing host allowlist already present in the codebase
- add a missing local authorization call when the required policy is already well-established and consistent nearby

Do not auto-fix without explicit approval when the change affects:

- authentication, authorization, or session semantics
- crypto, signing, token issuance, or password handling
- public API contracts or externally visible error behavior
- CORS, CSRF, rate limits, or webhook verification behavior
- infrastructure trust boundaries, proxies, or network topology assumptions
- database schema, tenant partitioning, or persistence format
- shared security middleware, global filters, or framework-wide defaults
- incident-sensitive paths where a partial fix could hide a broader problem

For those, report the issue and propose the smallest safe remediation path.

## Choose the smallest defensible fix

Prefer this order:

1. Re-establish the correct trust boundary.
2. Deny access or reject input earlier.
3. Replace unsafe sink usage with the existing safe abstraction.
4. Reduce secret and sensitive data exposure.
5. Leave broad redesigns as recommendations, not surprise edits.

Avoid speculative hardening churn. The point is to fix concrete security risk without inventing new behavior or breaking real controls.

## Report findings in this format

List findings first, highest severity first.

For each finding, include:

- priority: `P1` to `P4`
- vulnerability type: broken access control, weak validation, injection risk, secret exposure, unsafe file or process handling, network trust failure, cross-tenant leak, or insecure default
- location: file and line or the smallest concrete scope available
- attack path: how untrusted input, privilege, or data reaches the vulnerable behavior
- impact: what an attacker or unauthorized user could do
- recommended fix
- whether it is safe to auto-fix now

If no issues are found, say so explicitly and mention which trust boundaries and sensitive sinks were checked plus any residual blind spots.

## Default review stance

- Prefer explicit trust boundaries over implicit assumptions.
- Prefer deny-by-default over permissive fallback behavior.
- Prefer parameterization and allowlists over sanitization folklore.
- Prefer tenant and ownership scoping at every sensitive data boundary.
- Prefer concrete exploit paths or security regressions over vague hardening advice.
