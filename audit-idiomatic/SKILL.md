---
name: audit-idiomatic
description: Audit a codebase for non-idiomatic use of its detected languages and frameworks, including framework-convention bypasses, hand-rolled replacements for standard abstractions, lifecycle misuse, stringly-typed or weakly-modeled code, and patterns that fight the local stack instead of using it well. Use when reviewing a repository, feature, refactor, or bugfix for language or framework misuse, requests to make code more idiomatic, or requests to audit and safely auto-fix stack-specific code smells.
---

# Audit Idiomatic

Audit for stack fit before changing style. Find places where the code fights the language, runtime, or framework, then prefer the smallest idiomatic correction with clear payoff.

## Follow this workflow

1. Detect the active languages, frameworks, and major libraries in scope.
2. Establish the local idiomatic baseline from nearby code and project conventions.
3. Search for code that bypasses the stack's normal abstractions or misuses its lifecycle.
4. Separate real idiom violations from harmless preference differences.
5. Prioritize findings as `P1` through `P4`.
6. Auto-fix only local, behavior-preserving issues.

## Detect the stack first

Do not audit against generic internet style. Infer the actual stack from repository evidence.

Check sources like:

- `package.json`, `tsconfig.json`, lockfiles, and framework config
- `composer.json`, framework service providers, and base classes
- `pyproject.toml`, `requirements.txt`, and app entrypoints
- `Gemfile`, `go.mod`, `Cargo.toml`, and equivalent manifests
- imports, inheritance, decorators, annotations, hooks, macros, and DSLs
- test setup, lint config, formatting config, and existing shared helpers

Build a short mental map:

- primary language and version
- primary framework and rendering/runtime model
- data layer and validation approach
- async or concurrency model
- testing style already used in the repo

## Establish the idiomatic baseline

Prefer repository-local conventions when they are consistent and intentional.

Use this order:

1. established project pattern
2. framework convention
3. language standard library or core idiom
4. ecosystem best practice

Do not flag code merely because you would write it differently. Report only when the current approach creates concrete cost such as bugs, fragility, excess boilerplate, misuse of framework guarantees, or avoidable maintenance burden.

## Search for non-idiomatic patterns

Start with the language-appropriate equivalents of:

- custom parsing, validation, serialization, or routing
- manual state synchronization
- raw queries or direct persistence calls
- handwritten async orchestration
- reflection, dynamic maps, or `any`-style escape hatches
- duplicated conversion helpers
- lifecycle hooks, effects, callbacks, middleware, or observers
- custom collection helpers, loops, and reducers
- repeated string literals for typed concepts

Then inspect the surrounding code for the idiomatic alternative already present nearby.

## Detect these idiom violations

### Framework convention bypass

Flag code that sidesteps core framework primitives without a clear reason.

Common signals:

- hand-rolled request validation where the framework already owns it
- direct database or DOM access that bypasses the app's normal abstraction layer
- manual serialization, routing, dependency wiring, or form handling despite framework support
- custom state containers or fetch wrappers that duplicate the framework's established data flow

Prefer the framework path unless there is a measured requirement the framework cannot meet.

### Fighting the type or data model

Flag code that encodes structured concepts in weaker forms than the stack supports.

Common signals:

- stringly-typed status or mode values where enums, unions, or value objects already exist
- dictionaries, arrays, or loosely-shaped hashes standing in for well-defined domain types
- widespread casts, assertions, or `any`-style escapes that suppress the language's checks
- boolean parameter clusters that should be one explicit variant or configuration object

Prefer modeling the concept directly in the language's type system or the project's existing domain abstractions.

### Reimplementing built-ins or common abstractions

Flag code that recreates standard library, framework, or repository helpers with more risk and less clarity.

Common signals:

- manual mapping, filtering, grouping, or cloning where established primitives already exist
- custom error wrappers, response objects, or collection utilities duplicating shared helpers
- handwritten caching, retry, memoization, or event plumbing where the stack provides it
- repeated utility code copied across files instead of using the common abstraction

Prefer the smallest existing primitive that already communicates the intent.

### Lifecycle misuse

Flag side effects or cleanup logic placed in the wrong layer or hook.

Common signals:

- business logic embedded in view or controller lifecycle callbacks
- resource setup without matching teardown in the framework's normal lifecycle
- async work launched from places that do not own cancellation, retries, or error propagation
- persistence or network calls inside callbacks intended to stay pure or synchronous

Prefer the hook, boundary, or layer that owns the effect semantically.

### Cross-layer leakage

Flag code that ignores the responsibilities implied by the stack.

Common signals:

- controllers, components, or routes containing domain rules that belong in services or models
- models or entities performing UI formatting or transport shaping
- framework DTOs, request objects, or ORM models leaking deep into unrelated layers
- presentation code branching on persistence details that should have been normalized earlier

Prefer each layer's normal contract over convenience shortcuts.

### Local convention drift

Flag code that is technically valid but inconsistent with a strong project-local idiom in a way that raises maintenance risk.

Common signals:

- mixing two async styles, test styles, state-management approaches, or dependency patterns in one subsystem
- introducing a new helper pattern where the repository already has a stable shared approach
- code shaped unlike adjacent modules, making extension or debugging meaningfully harder

Only report this when the inconsistency has a real maintenance or correctness cost. Do not use this category for cosmetic style nits.

## Classify findings

### `P1`

Use for non-idiomatic code that breaks framework guarantees, causes incorrect lifecycle behavior, corrupts data flow, or creates bugs likely to surface in normal operation.

### `P2`

Use for code that materially raises defect risk or maintenance cost by bypassing core abstractions, weakening types, or introducing conflicting patterns in shared paths.

### `P3`

Use for code that is likely correct today but fights the stack enough to slow future work, obscure intent, or encourage repeated misuse.

### `P4`

Use for low-risk cleanup where an idiomatic replacement would improve clarity or consistency but the current code is unlikely to fail soon.

## Auto-fix only when safe

Auto-fix local issues when the idiomatic replacement is already established and behavior can stay the same.

Safe examples:

- replace duplicated local helper code with an existing shared primitive already used nearby
- convert repeated magic strings to an enum, constant, or typed helper that already exists in scope
- move a small side effect into the framework's standard hook when ownership is obvious
- replace a manual collection transform with the stack's standard primitive when the semantics match
- remove unnecessary casts or wrappers once the underlying typed path is already present

Do not auto-fix without explicit approval when the change affects:

- public API contracts
- schema or persistence format
- framework architecture across many files
- lifecycle or rendering semantics with uncertain edge cases
- dependency injection or service registration wiring
- concurrency, retries, transactions, or caching behavior
- established team conventions that may be deliberate but undocumented

For those, report the issue and propose the smallest migration path.

## Choose the smallest defensible fix

Prefer this order:

1. Reuse the repository's existing idiomatic abstraction.
2. Move logic to the layer or hook that already owns it.
3. Strengthen the model or type only as far as needed.
4. Remove custom plumbing before introducing new abstractions.
5. Leave broad rewrites as recommendations, not surprise edits.

Avoid style-driven churn. The point is to reduce stack friction, not to make every file look the same.

## Report findings in this format

List findings first, highest severity first.

For each finding, include:

- priority: `P1` to `P4`
- idiom gap: framework bypass, weak modeling, reimplemented abstraction, lifecycle misuse, cross-layer leakage, or local convention drift
- location: file and line or the smallest concrete scope available
- detected stack context: language, framework, or local pattern being violated
- why the current code is non-idiomatic in a way that matters
- smallest idiomatic fix
- whether it is safe to auto-fix now

If no issues are found, say so explicitly and mention which stack conventions and layers were checked plus any residual blind spots.

## Default review stance

- Prefer repository evidence over generic advice.
- Prefer framework guarantees over custom plumbing.
- Prefer explicit models over stringly or weakly-typed code.
- Prefer one consistent local pattern over mixed abstractions.
- Prefer findings with concrete cost over stylistic opinions.
