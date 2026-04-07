---
name: finalise-plan
description: "Carefully review the current implementation or execution plan for holes, mistakes, weak assumptions, missing dependencies, bad sequencing, and inadequate validation, then update the plan to reflect the findings. Use when a plan has just been drafted, revised, or is about to be executed and needs a thorough quality pass."
---

# Finalise Plan

Review the current plan like a critical peer review. Find the real problems, then update the plan so the fixes are captured in the plan itself.

## Workflow

1. Read the full request, surrounding context, and current plan state before judging the plan.
2. Look for holes:
   - missing scope boundaries
   - missing discovery or dependency checks
   - missing implementation steps
   - missing validation or rollout steps
   - missing final reporting or follow-up work
3. Look for mistakes:
   - incorrect sequencing
   - steps that are too broad to execute safely
   - assumptions presented as facts
   - hidden blockers, prerequisites, or irreversible actions
   - acceptance criteria that are vague or untestable
4. If anything material is still unclear, ask the user to clarify with the ask questions tool before finalizing the plan.
5. Update the plan directly to reflect the findings:
   - add missing steps
   - split broad steps into atomic work
   - reorder steps to respect dependencies
   - ensure each task is sized for a single atomic commit
   - add explicit validation tasks
   - document unresolved risks or assumptions when they cannot yet be removed
6. If the findings show the plan needs a substantial rewrite rather than small corrections, revise the plan with the [$planner](/Users/SebKay/.agents/skills/planner/SKILL.md) skill.
7. Keep plan statuses coherent and ensure only one item is `in_progress`.
8. After updating the plan, briefly summarize the key corrections and any remaining open risks.

## Review Standard

- Prefer fixing the plan over merely describing problems.
- You MUST make an atomic commit for each task in the plan, and revise the plan if any task is too large or mixed to support that.
- Escalate to the [$planner](/Users/SebKay/.agents/skills/planner/SKILL.md) skill when the plan needs to be restructured, expanded into phases, or rebuilt from weak foundations.
- Be skeptical of plans that jump straight to implementation.
- Treat missing validation as a real defect.
- Treat unclear assumptions as defects unless they are explicitly called out.
- Ask only clarifying questions that materially change the plan.
- Stop once the plan is concrete, sequenced, and testable.
