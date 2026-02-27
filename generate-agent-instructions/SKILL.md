---
name: generate-agent-instructions
description: "Help generate or update the AGENTS.md file for guiding AI coding agents."
---

Analyze this codebase to generate or update `AGENTS.md` for guiding AI coding agents.

**Important**: You can completely ignore the CLAUDE.md file, if it exists, as it's a duplicate of AGENTS.md.

Do a full deep dive into the codebase so AI agents always have enough context to prevent them making assumptions and mistakes.

Focus on discovering the essential knowledge that would help an AI agent be immediately productive in this codebase. Consider aspects like:
- The "big picture" architecture that requires reading multiple files to understand - major components, service boundaries, data flows, and the "why" behind structural decisions
- Critical developer workflows (builds, tests, debugging) especially commands that aren't obvious from file inspection alone
- Project-specific conventions and patterns that differ from common practices
- Integration points, external dependencies, and cross-component communication patterns

Source existing AI conventions from `AGENTS.md`.

Guidelines:
- If `AGENTS.md` exists, merge intelligently - preserve valuable content while updating outdated sections
- Write concise, actionable instructions (~20-50 lines) using markdown structure
- Include specific examples from the codebase when describing patterns
- Avoid generic advice ("write tests", "handle errors") - focus on THIS project's specific approaches
- Document only discoverable patterns, not aspirational practices
- Reference key files/directories that exemplify important patterns

Update `AGENTS.md` for the user, then ask for feedback on any unclear or incomplete sections to iterate.
