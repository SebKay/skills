---
name: review-code
description: "Review code changes for correctness, regressions, maintainability, and test coverage. Use when reviewing a feature, pull request, diff, refactor, or when the user asks for a code review."
---

# Review Code

Review with a code-review mindset. Findings come first; summaries are secondary.

## Workflow

1. Read the requested files, diff, or feature context first.
2. Trace the main behavior and the highest-risk edge cases.
3. Identify concrete issues that could cause bugs, regressions, security problems, UX issues, or hard-to-maintain code.
4. Check whether tests cover the risky behavior and call out missing coverage.
5. Report only findings you can justify from the code, the diff, or clearly stated assumptions.

## What To Look For

- Broken logic, unreachable code paths, missing states, and edge-case failures.
- Behavioral regressions in existing flows, APIs, or data contracts.
- Excessive branching, boolean flags, or conditional complexity that should be modeled more explicitly.
- Repetition or coupling that should be extracted into shared helpers, services, hooks, or components.
- Frontend code that should be split into reusable components or UI primitives.
- Error handling gaps, silent failures, race conditions, and async hazards.
- Performance problems in hot paths, unnecessary renders, redundant queries, or avoidable work.
- Security, validation, authorization, and data-handling issues.
- Missing, weak, or misleading tests.

## Findings Bar

- Prefer a few high-signal findings over many low-value suggestions.
- Focus on defects, risks, regressions, and missing tests before style or polish.
- Call out maintainability concerns when they are likely to create future defects or slow down changes.
- Skip speculative feedback unless you can explain the concrete risk.

## Response Format

List findings first, ordered by severity.

For each finding, include:

- Severity
- Short title
- Why it matters
- Relevant file or code path
- Suggested fix or direction when obvious

After the findings, include:

- Open questions or assumptions
- A brief change summary only if it adds value
- Residual risks or testing gaps if confidence is limited

## If No Findings

Say explicitly that no concrete issues were found.

Still mention any residual risk, untested paths, or assumptions that limited confidence.

## Review Style

- Be direct, specific, and evidence-based.
- Do not dilute important issues with praise or filler.
- Do not turn the review into a rewrite unless the current approach is risky.
- Prioritize actionable feedback over generic best-practice commentary.
