---
name: generate-laravel-agent-instructions
description: "Generate or update `.ai/guidelines/project.md` for Laravel codebases by extracting architecture, domain logic, jobs, workflows, and integration details from repository evidence. Use when the user asks to create, refresh, or improve Laravel-specific agent instructions or project overview guidance."
---

Generate or update `.ai/guidelines/project.md` using evidence from the current Laravel repository.

`.ai/guidelines/project.md` is pulled into `AGENTS.md`, so avoid duplicating guidance already present in `AGENTS.md` unless needed for local clarity.

## Workflow

1. Locate existing `.ai/guidelines/project.md` and use it as the base when present.
2. Inspect the codebase deeply enough to capture:
   - Core business and domain logic.
   - Laravel architecture and boundaries (routes, controllers, services/actions, models, policies).
   - Background processing (jobs, queues, listeners, notifications, scheduled tasks).
   - External integrations and cross-component data flow.
3. Extract only repository-backed facts from code, config, migrations, and docs.
4. Merge intelligently with the existing file: keep valid guidance, remove stale guidance, fill important gaps.
5. Write concise, actionable instructions for future coding agents.

## Output Requirements

- Keep the file under 100 lines.
- Prefer project-specific guidance over generic Laravel advice.
- Include concrete file or directory references for critical behavior.
- Prioritize domain rules, side effects, async flows, and failure-sensitive logic.
- Document current-state behavior only.

## Quality Bar

- Avoid assumptions not supported by repository evidence.
- Capture details that prevent costly mistakes in future agent runs.
- Favor brevity, but stay explicit on high-impact logic.
