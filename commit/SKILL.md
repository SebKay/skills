---
name: commit
description: "Create a focused git commit for the unit of work just completed. Use when the user asks to commit changes, record finished implementation work, save a fix or refactor to git, or prepare a clean commit from the current branch with a concise message."
---

# Commit Changes

Create one focused commit for the work that is ready now.

## Workflow

1. Inspect the working tree with `git status --short`.
2. Identify the files and hunks that belong to the requested unit of work.
3. Leave unrelated, pre-existing, or uncertain changes unstaged. If the scope is ambiguous, ask before committing.
4. Stage only the relevant files or hunks.
5. Review the staged diff with `git diff --cached --stat` and `git diff --cached`.
6. Write a concise imperative subject line that describes the change.
7. Create a single commit with no body unless the user explicitly asks for one.

## Guardrails

- Prefer the user's requested commit message when they provide one.
- Do not create empty commits.
- Do not use `--no-verify`, amend an existing commit, or bundle unrelated changes unless the user explicitly asks.
- If hooks or commit validation fail, surface the failure instead of bypassing it.
