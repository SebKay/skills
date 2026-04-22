---
name: ask-plan-questions
description: "Identify the minimum high-impact clarifying questions needed to make a plan safe to write or execute. Use when drafting, reviewing, or revising a plan; when scope, constraints, dependencies, acceptance criteria, rollout expectations, or non-negotiable behavior are unclear; or when the next implementation step depends on an unresolved assumption."
---

# Ask Plan Questions

Surface only the missing information that materially changes the plan. Reduce ambiguity without turning planning into an interview.

Use the `AskQuestion` tool for every question.

Do not force a fixed number of questions. Ask only what changes the plan, and stop once the remaining uncertainty can be handled by explicit assumptions.

If no plan exists yet, ask only enough to draft a safe first plan.

## Workflow

1. Read the request, thread, current plan, and nearby files before asking anything.
2. List the unresolved assumptions that could change scope, sequencing, design, testing, or rollout.
3. Rank them by impact:
   - blockers that could invalidate the plan
   - questions that affect architecture or sequencing
   - questions that affect validation or rollout
   - lower-risk details
4. Ask the highest-impact question first.
5. Ask one question at a time by default.
6. Ask a batch of 2-3 only when the questions are independent and the user can answer them together without confusion.
7. Update the plan or assumptions after each answer before asking anything else.
8. Stop when the remaining gaps are low-risk or can be handled with clearly stated assumptions.

## Ask vs Assume

Ask when the answer changes implementation decisions, sequencing, acceptance criteria, or risk.

Do not ask when:

- the answer is already in the repo, thread, or current plan
- a reasonable assumption is low-risk and easy to document
- the question is a pure preference and does not affect architecture or done criteria
- the question can wait until after the next safe step

When you assume, state the assumption plainly in the plan.

## High-Leverage Question Order

Prefer this order unless project context makes a different order safer:

1. Success criteria and definition of done
2. Scope boundaries and explicit non-goals
3. Non-negotiable constraints: stack, versions, interfaces, deadlines, approvals
4. Existing behavior that must not change
5. Fixed schemas, APIs, contracts, or external dependencies
6. Validation requirements: tests, environments, evidence
7. Rollout, backward-compatibility, migration, or fallback expectations
8. Edge cases, failure modes, security, or privacy constraints

## Question Selection Rules

- Ask only questions that change implementation decisions.
- Skip questions already answered in the thread or files.
- Prefer specific questions over broad prompts.
- Tie each question to one risk area.
- De-prioritize style preferences unless they affect architecture or acceptance.
- Prefer choices or bounded prompts when the likely options are clear.
- Avoid stacking multiple decisions into one question.

## Question Bank (pick only relevant items)

Use these as starting points. Reword them for the actual plan and drop anything that does not matter.

1. What outcome will make this task done?
2. What is explicitly out of scope?
3. Which existing behavior or interfaces must remain unchanged?
4. Which environments must this work in?
5. What technical constraints are fixed: language, framework, versions, hosting, tooling?
6. Which schemas, APIs, or contracts are fixed versus negotiable?
7. What dependencies, prerequisites, or sequencing constraints already exist?
8. What acceptance tests or checks must pass before this is considered done?
9. What rollout or migration strategy is expected?
10. What backward-compatibility requirements apply?
11. What edge cases or failure modes matter most?
12. What security, privacy, or compliance constraints apply?
13. When docs, code, and conversation disagree, what is the source of truth?

## Completion Criteria

Finish questioning when all of the following are true:

- Success criteria are testable.
- Scope boundaries are clear.
- Non-negotiable constraints and dependencies are explicit.
- Plan-shaping risks are resolved or documented as assumptions.
- The next planning or implementation step can proceed safely.
