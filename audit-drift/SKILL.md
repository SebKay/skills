---
name: audit-drift
description: Find state-management bugs across frontend, backend, and persistence layers by auditing impossible state combinations, boolean explosion, magic strings, duplicated or derived state, and source-of-truth violations. Use when reviewing reducers, stores, components, forms, API handlers, jobs, models, or database-backed workflows for inconsistent state, fragile transitions, drift between layers, or requests to audit and auto-fix state bugs.
---

# Audit Drift

Audit state before changing behavior. Find contradictions, drift, and weak state modeling first, then apply the smallest safe fix.

## Follow this workflow

1. Inventory every state surface involved in the feature or bug.
2. Mark the source of truth for each state value.
3. Trace how state is written, derived, cached, serialized, and invalidated.
4. Identify impossible combinations, duplicated state, and uncontrolled transitions.
5. Prioritize findings as `P1` through `P4`.
6. Auto-fix only local, low-risk issues. Leave wider schema or contract changes as findings with a concrete fix plan.

## Inventory state surfaces

Check the full path, not just the visible bug site.

- UI component state
- Form state
- URL params
- Client stores and reducers
- Query/cache layers
- API request and response payloads
- Background job payloads
- Domain models and services
- Database columns and denormalized fields
- Feature flags and config

Use search terms like:

- `status`
- `state`
- `phase`
- `step`
- `mode`
- `type`
- `kind`
- `is_`
- `has_`
- `can_`
- `current`
- `selected`
- `pending`
- `loading`
- `error`
- `success`

## Detect these drift patterns

### Impossible state combinations

Find combinations the business flow should never allow.

Examples:

- `isLoading === true` and `data !== null` and `error !== null`
- `status === 'paid'` with `paid_at === null`
- `isArchived === true` while `status === 'active'`
- `currentStep = 4` while prerequisite step state is incomplete

Prefer one explicit state field over multiple loosely-related booleans.

### Boolean explosion

Flag clusters of booleans that encode a hidden state machine.

Common signals:

- three or more booleans governing one workflow
- repeated guard clauses combining the same flags in different ways
- flags that are mutually exclusive but not enforced

Prefer:

- enum or discriminated union
- explicit transition map
- state machine when transitions are complex

### Magic strings

Flag ad-hoc string literals used as state values across multiple files.

Common signals:

- `'draft'`, `'active'`, `'done'` repeated in controllers, UI, tests, and jobs
- inconsistent spellings like `'in_progress'`, `'in-progress'`, `'processing'`
- string comparisons spread across conditionals with no central definition

Prefer shared constants, enums, value objects, or typed unions.

### Duplicated or derived state

Find values stored in multiple places when one copy is already derivable.

Common signals:

- `fullName` stored next to `firstName` and `lastName`
- `itemCount` stored next to `items.length`
- `isOverdue` stored next to `dueAt`
- local component state copying server cache or props and drifting later

Prefer deriving the value at read time unless there is a measured performance reason not to.

### Source-of-truth violations

Find multiple writers for the same conceptual fact.

Common signals:

- UI mutates status optimistically while backend recomputes it differently
- reducer state duplicates URL state
- database column caches a value but update paths miss some writers
- API response includes both canonical and legacy state fields with mismatched meanings

Require one authoritative owner and clearly-defined derivations from it.

## Classify findings

### `P1`

Use for bugs that can cause broken workflows, invalid persistence, security or billing mistakes, silent data corruption, or user-visible contradictions that normal usage can hit.

### `P2`

Use for drift that may not fail immediately but can diverge in normal operation, especially duplicated state with multiple writers or hidden transition rules.

### `P3`

Use for modeling weaknesses that raise regression risk: boolean explosion, magic strings, partially-centralized enums, weak guards, or missing invariants.

### `P4`

Use for cleanup work with low immediate risk: dead state, redundant aliases, naming drift, or minor consolidation opportunities.

## Auto-fix only when safe

Auto-fix local issues when the change is mechanical and the source of truth is unambiguous.

Safe examples:

- replace repeated state literals with a local constant or shared enum already implied by the code
- remove obviously duplicated derived state inside one module
- collapse redundant conditionals into one canonical predicate
- add guard clauses or assertions for impossible local combinations
- rename inconsistent local state keys when usage is contained

Do not auto-fix without explicit approval when the change affects:

- database schema
- persisted data shape
- public API contracts
- cross-service message formats
- broad architectural rewrites
- migrations between booleans and enums that span many files

For those, report the finding and propose the smallest migration path.

## Choose the smallest defensible fix

Prefer this order:

1. Remove duplicated state.
2. Derive values from the canonical source.
3. Replace boolean clusters with one explicit status field.
4. Centralize state constants and transition rules.
5. Add invariant checks where invalid states must fail fast.

Avoid speculative rewrites. Fix the drift closest to the real source-of-truth problem.

## Check transitions, not just snapshots

A state model is only correct if transitions are controlled.

Inspect:

- who can write each state value
- whether writes are atomic
- whether async paths can reorder updates
- whether retries or optimistic updates can reintroduce stale state
- whether cleanup paths reset all related fields
- whether serialization drops information needed to reconstruct the state safely

## Report findings in this format

List findings first, highest severity first.

For each finding, include:

- priority: `P1` to `P4`
- drift type: impossible state, boolean explosion, magic strings, duplicated state, derived state, or source-of-truth violation
- location: file and line or the smallest concrete scope available
- why it can drift
- user or system path that triggers it
- recommended fix
- whether it is safe to auto-fix now

If no issues are found, say so explicitly and mention which state surfaces were inspected plus any residual blind spots.

## Default review stance

- Prefer correctness over style
- Prefer one source of truth over convenience copies
- Prefer explicit transitions over implicit flag combinations
- Prefer contained fixes over broad rewrites
- Prefer findings with concrete failure paths over abstract concerns
