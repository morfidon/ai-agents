# AGENTS.md

## Purpose

This repository may be edited by both humans and AI agents.

The goal is not just to make code "work".
The goal is to make changes that are:

- safe
- local
- easy to review
- easy to test
- easy to understand without tribal knowledge

When in doubt, choose the option that is more explicit, more boring, and easier to verify.

## Core principle

Optimize for **safe change**.

A good change is one that a competent stranger could understand by reading:

- the file being changed
- its direct types or contracts
- one or two nearby tests

If a change requires hidden context, framework magic, or assumptions that are not visible in code, it is risky.

## Default style

Prefer:

- explicit code over clever code
- composition over inheritance
- local logic over shared magic
- strong contracts over loose objects
- small modules over generic engines
- obvious names over abstract names
- loud failure over silent fallback

Avoid introducing abstractions unless they solve a real, current problem.

## What AI agents should optimize for

When making changes, optimize for:

1. Correctness
2. Safety
3. Clarity
4. Small blast radius
5. Easy rollback
6. Easy review

Do not optimize for elegance alone.

## Preferred patterns

### 1. Keep logic local

Prefer:

- small files
- small functions
- one responsibility per module
- behavior close to where it is used

Avoid:

- giant utility files
- central "smart" layers used by many unrelated features
- helpers with broad hidden impact

### 2. Prefer explicit wiring

Prefer:

- direct imports
- explicit parameters
- visible configuration
- clear registration

Avoid:

- service locator patterns
- hidden dependency injection
- auto-discovery by naming convention alone
- reflection-heavy registration
- magic framework behavior for core business rules

### 3. Prefer composition over inheritance

Prefer:

- small helpers
- wrapper modules
- dependency injection through parameters
- plain objects and functions

Avoid:

- deep inheritance trees
- base classes with important hidden behavior
- multiple layers of overrides

### 4. Keep contracts strict

Prefer:

- strong typing
- schemas
- explicit input validation
- stable return shapes
- clearly named fields

Avoid:

- loose bags of data
- context-dependent return shapes
- optional fields that are actually required
- data that changes shape silently

### 5. Make side effects obvious

Prefer:

- functions that either compute or mutate
- names that reveal writes and external actions
- explicit transaction boundaries
- clear read vs write paths

Avoid:

- hidden writes
- mixed compute + save + notify + cache in one helper
- misleading names that sound read-only but mutate state

### 6. Keep state ownership clear

Prefer:

- one source of truth
- explicit state flow
- passed-in state
- narrow ownership

Avoid:

- global mutable state
- mutable singletons
- competing sources of truth
- ambient state that can change from many places

### 7. Use boring names

Prefer names like:

- `createInvoiceDraft`
- `getTenantBySlug`
- `isUserAllowedToDeleteProject`

Avoid names like:

- `handleData`
- `processThing`
- `manager`
- `helper`
- `doStuff`

Names should describe real behavior, not vague intent.

## Patterns to avoid unless there is a strong reason

These patterns are not forbidden, but they are risky in AI-edited codebases:

- deep inheritance
- metaprogramming-heavy designs
- reflection-heavy behavior
- generic engines shared across unrelated workflows
- broad shared utility layers
- hidden side effects
- silent fallback defaults for critical config
- convention-only registration without explicit mapping
- business logic buried in framework lifecycle hooks
- abstractions created for hypothetical future reuse

If you use one of these, document why.

## Refactoring rules

Refactor only when at least one of these is true:

- the current structure already caused bugs
- the current structure makes safe change difficult
- the current structure hides boundaries, auth, validation, or state flow
- the refactor reduces coupling or blast radius in a measurable way
- the refactor makes tests easier and stronger

Do not refactor just to make code feel cleaner.

Do not combine refactor + feature + rename + cleanup in one change unless truly necessary.

## Change scope rules

Keep changes narrow.

A single change should ideally do one thing:

- fix one bug
- add one feature
- perform one refactor
- improve one contract
- add one test set for one behavior

Avoid mixed-purpose changes.

If a rename causes massive noise, separate it from behavior changes if possible.

## Rules for editing existing code

Before changing code:

1. Identify the exact responsibility of the module.
2. Identify direct callers and direct dependencies.
3. Identify whether the code is on a security, auth, filesystem, persistence, billing, or state boundary.
4. Check for nearby tests.
5. Preserve existing contracts unless the task explicitly changes them.

When changing code:

- keep existing behavior unless change is required
- do not "clean up" unrelated code in the same patch
- do not silently change return shapes
- do not weaken validation
- do not move auth checks farther away from mutation
- do not replace explicit code with magic

After changing code:

- verify all affected callers
- add or update tests
- explain the risk and the reason for the change

## Boundary protection rules

Be extra careful around:

- authentication
- authorization
- filesystem access
- path handling
- database writes
- transactions
- money flows
- billing
- tenant isolation
- secrets
- background jobs
- caching
- concurrency
- idempotency

For these areas:

- validate at the boundary
- keep checks close to the mutation
- fail loudly on invalid state
- avoid hidden fallbacks
- preserve transactional consistency
- prefer explicit code paths over abstraction

## Failure handling rules

Prefer:

- explicit errors
- clear logs
- input validation
- invariant checks
- fail-fast behavior for invalid critical state

Avoid:

- catch-and-ignore
- silent fallback to empty values for secrets
- swallowing validation failures
- continuing after broken assumptions

Invalid state should be visible.

## Testing rules

Every risky change should come with evidence.

Preferred evidence:

- focused test added or updated
- reproduction steps
- before/after contract explanation
- caller audit
- dependency trace
- transaction or state reasoning when relevant

Prefer tests that verify behavior.

Avoid tests that only mirror implementation detail unless that detail is the contract.

Prioritize tests around:

- boundaries
- regressions
- data contracts
- auth
- persistence
- state synchronization
- race-prone flows

## Documentation rules

Critical modules should contain a short contract note in code comments or nearby docs that explains:

- what the module owns
- what it must not do
- key invariants
- common mistakes
- related tests

Example:

```txt
This module owns tenant path resolution.
Always use TenantPathResolver for tenant filesystem paths.
Do not build tenant paths manually.
Related tests: tests/TenantPathResolverTest.php
```
