Goal:List code changes that likely slow the app. No edits. No execution.

Method:

Read diffs and changed files.

Compare new code to known slow shapes.

Flag structural costs before any timing runs.

Group by file and function or query.

What to Look For:

Added nested loops

New loop inside another loop.

Loop over network or disk calls.

Bigger queries and joins

New JOINs, GROUP BY, DISTINCT on large tables.

SELECT * added where few fields are used.

Cache removed or bypassed

Deleted memoization.

Removed HTTP/database cache layer.

Hot-path allocations

New object or string creation inside tight loops.

N+1 risks introduced

Query placed inside an iteration.

Heavier serialization

New JSON encode/decode in loops.

Sync I/O on hot path

File or network calls added to request cycle.

Wider data loads

Fetching full records instead of projections.

Unbounded sorts/filters

Sorting full collections, then slicing.

Removed batching

Single-item calls replace prior bulk ops.

Increased cardinality metrics

New high-cardinality labels in metrics.

Broader locks

Critical section now wraps more work.

Heavier regex or parsing

Complex regex in per-request code.

Raised concurrency without guards

More parallel tasks that fight for the same resource.

Wider result sets in APIs

Higher default page size with no server pagination.

Expected Output Format:Readable. One line per finding. Grouped by file and symbol.

File: src/service/orders.ts
  - Function: computeTotals()
    Drift: New nested loop over items inside customers loop (O(n^2) risk)
    Confidence: Medium
    Severity: Major

File: db/queries/report.sql
  - Query: monthly_revenue
    Drift: Added JOIN to line_items and GROUP BY month - large table, likely slower
    Confidence: Medium
    Severity: Major

File: src/cache/totalsCache.ts
  - Module: totalsCache
    Drift: Removed memoize() wrapper around computeTotals
    Confidence: High
    Severity: Major

File: api/users/repo.js
  - Function: listUsers()
    Drift: SELECT * introduced; UI uses id,email only
    Confidence: Medium
    Severity: Moderate

File: src/http/client.ts
  - Function: fetchProfile()
    Drift: JSON.parse in a loop for each record
    Confidence: Medium
    Severity: Moderate

File: web/metrics/prom.ts
  - Metric: http_requests_total{userId}
    Drift: Added userId label - high cardinality risk
    Confidence: Medium
    Severity: Major


Output Rules:

List every structural slowdown you see.

Include file, symbol (function/query/module/metric), short drift note, confidence, and severity.

Do not propose patches. Do not time code.

Sort by file path, then symbol, then line if known.

Use exact names from the diff.

Severity:

Major for added nested loops, heavy joins, cache removal, high-cardinality metrics, sync I/O on hot paths.

Moderate for wider selects, per-item parsing, and removed batching where load seems smaller.

Confidence:

Medium by default – heuristic pattern match.

High when a cache layer is clearly removed or a query obviously expands.

Low only when the diff is ambiguous about data size or hot path status.
