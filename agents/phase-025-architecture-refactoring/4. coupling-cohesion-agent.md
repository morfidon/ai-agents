Goal:Cut tight links. Group code that belongs together. No edits. No execution.

Method:

Read each module’s public API.

Read who calls whom inside.

Count fan-in and fan-out.

Note clusters that always travel together.

Group findings by module.

What to Look For:

Tight coupling

Module imports many others to do one task.

Changes in one file force changes in many files.

Wide fan-in hubs

Many modules depend on one unstable helper.

Wide fan-out spokes

One module depends on many unrelated modules.

Low cohesion

Module exposes many unrelated functions.

Public API mixes concerns.

Feature scattered

One feature’s flow jumps across many small files.

Utility dumping ground

utils or common folder with everything inside.

Leaky abstractions

Callers know too much about internals.

Chatty neighbors

Two modules call each other many times per flow.

Barrel re-export bloat

index.ts exposes more than needed.

DTO and helper clones

Same shapes repeated in many modules.

Heuristics:

Fan-in: how many modules import this one.

Fan-out: how many modules this one imports.

API width: number of public symbols.

Change surface: files in the same commits often.

Name hints: utils, common, shared.

Expected Output Format:Readable. One line per finding. Grouped by module.

Module: src/orders/PriceEngine.ts
  Note: High fan-out - imports 11 modules for one compute flow
  Severity: Major
  Confidence: Medium

Module: src/shared/utils/index.ts
  Note: Utility dumping ground - 38 exports across unrelated domains
  Severity: Major
  Confidence: Medium

Module: packages/catalog/src/index.ts
  Note: Barrel re-exports all internals - public API too wide
  Severity: Moderate
  Confidence: Medium

Module: src/payments/GatewayAdapter.ts
  Note: Chatty neighbors - tight loop of calls with RetryPolicy.ts
  Severity: Moderate
  Confidence: Medium

Module: src/users/profile/
  Note: Feature scattered - read, write, validate split across 7 tiny files
  Severity: Moderate
  Confidence: Medium

Module: src/core/strings.ts
  Note: Wide fan-in hub but unstable - many callers break on small changes
  Severity: Major
  Confidence: Medium


Output Rules:

List every coupling or cohesion issue.

Include module, short note, Severity, Confidence.

Do not propose refactors.

Sort by path.

Use exact module names you see.

Severity:

Major for high fan-in hubs that change often, high fan-out spokes in core flows, and giant utility dumps.

Moderate for scattered features, wide barrels, and chatty pairs.

Confidence:

Medium by default - structure based.

Raise to High if fan-in or fan-out is extreme or confirmed by commit history.

Lower only if module boundaries are unclear.
