---
name: finalise-plan
description: "Carefully review the current implementation or execution plan for holes, mistakes, weak assumptions, missing dependencies, bad sequencing, and inadequate validation, then update the plan to reflect the findings. Use when a plan has just been drafted, revised, or is about to be executed and needs a thorough quality pass."
---

# Finalise Plan

Review the current plan like a critical peer review. Find the real problems, then update the plan so the fixes are captured in the plan itself.

## Commit Discipline

Treat per-task commits as an execution constraint, not a reporting preference.

- Every implementation task in the plan MUST be small enough to be completed, validated, and committed independently.
- Every implementation task MUST include:
  - `Commit:` the exact commit boundary for that task
  - `Validation:` the checks that must pass before committing
- The implementer MUST stop after each completed task, run that task’s validation, and create the commit before beginning the next task.
- If work for multiple tasks accumulates in the same uncommitted worktree, that is a plan failure. Revise the plan into smaller tasks first.
- Do not defer commits until the end of implementation.
- If a task cannot realistically be committed independently, it is too large or incorrectly scoped and the plan MUST be revised before execution.

## Workflow

1. Read the full request, surrounding context, and current plan state before judging the plan.
2. Look for holes:
   - missing scope boundaries
   - missing discovery or dependency checks
   - missing implementation steps
   - missing validation or rollout steps
   - missing final reporting or follow-up work
   - missing commit checkpoints or unclear commit boundaries
3. Look for mistakes:
   - incorrect sequencing
   - steps that are too broad to execute safely
   - assumptions presented as facts
   - hidden blockers, prerequisites, or irreversible actions
   - acceptance criteria that are vague or untestable
   - tasks that cannot be validated and committed independently
   - plans that imply batching multiple tasks into one commit
4. If anything material is still unclear, ask the user to clarify with the ask questions tool before finalizing the plan.
5. Update the plan directly to reflect the findings:
   - add missing steps
   - split broad steps into atomic work
   - reorder steps to respect dependencies
   - add explicit validation tasks
   - add explicit per-task commit checkpoints
   - document unresolved risks or assumptions when they cannot yet be removed
6. After updating the plan, briefly summarize the key corrections and any remaining open risks.
7. Revise the plan using the `/planner` skill.
8. You MUST structure the plan so each task ends in its own validation checkpoint and atomic commit, and the implementer must commit immediately after completing that task.

## Review Standard

- Prefer fixing the plan over merely describing problems.
- You MUST structure the plan so each task ends in its own validation checkpoint and atomic commit.
- Treat tasks without an explicit validation-and-commit boundary as defective.
- Treat deferred end-of-work batching commits as a sequencing error, not a style choice.
- You MUST make an atomic commit for each task in the plan, and revise the plan if any task is too large or mixed to support that.
- Be skeptical of plans that jump straight to implementation.
- Treat missing validation as a real defect.
- Treat unclear assumptions as defects unless they are explicitly called out.
- Ask only clarifying questions that materially change the plan.
- Stop once the plan is concrete, sequenced, and testable.
