---
name: finalise-plan
description: "Stress-test implementation, execution, rollout, migration, or task plans for missing steps, bad sequencing, weak assumptions, hidden dependencies, vague acceptance criteria, and inadequate validation. Use when a plan has just been drafted or revised, when inheriting someone else's plan, before starting implementation, or when a plan needs one last quality pass before execution."
---

# Finalise Plan

Pressure-test the plan, then repair it. Do not stop at critique if you can make the plan concrete and safer yourself.

## Review the plan in context

1. Read the user request, surrounding discussion, and current plan before changing anything.
2. Identify the plan artifact to update. Prefer revising the existing plan in place instead of producing a separate review unless the user asked for findings only.
3. If key context is missing and would materially change the plan, ask the user. Otherwise, record the assumption in the plan and continue.

If the workspace already has a standard plan structure, preserve it instead of inventing a new format.

## Look for plan defects

Check for:

- missing scope boundaries, prerequisites, discovery work, or dependency checks
- incorrect sequencing or tasks that jump past unresolved blockers
- tasks that are too broad, mixed, or not independently verifiable
- acceptance criteria or validation steps that are vague, missing, or impossible to run
- hidden irreversible actions, rollout risks, or missing cleanup and follow-up work
- assumptions stated as facts
- places where ownership, ordering, or gating rules are still unclear

## Repair the plan

Update the plan directly to fix what you found:

- add missing discovery, implementation, validation, rollout, or reporting steps
- reorder tasks to respect dependencies
- split broad tasks into atomic units
- make acceptance criteria and validation explicit
- document unresolved risks or assumptions when they cannot yet be removed

## Enforce execution boundaries

For implementation plans, every task should be small enough to complete, validate, and commit independently.

Each implementation task should state:

- `Validation:` what must pass before the task is done
- `Commit:` the exact boundary that should be committed immediately after validation

If a task cannot support an independent validation and commit boundary, revise the plan into smaller tasks first.

For non-code plans, apply the same rule using the smallest independently verifiable completion boundary available.

## Finish with a quality check

Before stopping, confirm the revised plan is:

- concrete instead of aspirational
- sequenced against real dependencies
- explicit about assumptions and open risks
- testable at the task level
- safe to execute without batching unrelated work

Then give a brief summary of the key corrections and any remaining open risks.

## Default stance

- prefer fixing the plan over merely describing problems
- prefer the smallest plan changes that remove real risk
- prefer explicit assumptions over hidden ones
- prefer validation as part of the plan, not a postscript
- prefer atomic tasks over broad phases when execution details matter
- stop when the plan is concrete, sequenced, and testable
