---
name: generate-laravel-project-overview
description: "Help generate or update the .ai/guidelines/project.md overview file with anything not covered."
---

Analyze this codebase to generate or update `.ai/guidelines/project.md` for guiding AI coding agents in the latest state of the project.

`.ai/guidelines/project.md` is automatically pulled into `AGENTS.md` so don't duplicate anything from `AGENTS.md` that isn't already in `.ai/guidelines/project.md`.

It should cover everything the project does, so things like business logic and domain logic are very important, as well as things like queued jobs.

Try to be concise and make the file no longer than 100 lines. Favour brevity, but be explicit.
