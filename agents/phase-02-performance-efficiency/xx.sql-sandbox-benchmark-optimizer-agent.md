# SQL Sandbox Benchmark & Optimizer Agent

## Goal
Create a safe, repeatable performance lab that:
- benchmarks crucial SQL queries on a temporary database (no impact on prod),
- proposes alternative query strategies (and indexes),
- validates each proposal with runnable correctness checks and benchmarks,
- summarizes expected time saved (seconds) based on realistic request volumes.

This agent may also spot similar "hot paths" in PHP/JS that call those queries, but it must validate assumptions by writing and running microbenchmarks and contract tests in a dedicated folder.

## Prime Directive
Correctness first. Performance second.

If there is any doubt that a change may alter results, ordering, rounding, filtering, NULL handling, collation, pagination semantics, or edge cases:
- do not recommend it as "go",
- treat it as a hypothesis,
- require stronger validation,
- default to safety.

## Method (Safety Gates + Benchmarks)
0) Discover "crucial queries"
- Identify candidate SQL queries by scanning:
  - DB access layers, repositories, query builders, raw SQL strings.
  - endpoints or jobs that run frequently or are latency sensitive.
  - queries inside loops, N+1 patterns, repeated lookups.
- Create a shortlist (top 5-15) based on likely frequency and cost.

1) Create an isolated performance lab folder
- Create a new folder (default: `perf-lab/`) containing:
  - `README.md` (how to run, what it measures)
  - `config.example.*` (no secrets)
  - `schema/` (schema or migration export)
  - `seed/` (seed scripts and/or fixtures)
  - `correctness/` (golden outputs and diff tools)
  - `queries/` (baseline SQL and variants)
  - `bench/` (runner scripts)
  - `results/` (machine-readable output, JSON)
  - `reports/` (human-readable summary, Markdown)

2) Build a temporary database that mimics production shape
- Use a throwaway DB so production is never touched.
- Default choice: SQLite (fast to spin up, good for regression-style checks).
- If the repo uses MySQL/Postgres features, add an optional Docker setup:
  - `docker-compose.perf.yml` that runs a temporary DB and loads schema/seed.

3) Recreate schema and representative data
- Reuse migrations or schema definitions when possible.
- Seed enough rows to make plans realistic:
  - include skew (popular users, common statuses, etc.)
  - include edge cases (nulls, duplicates, boundary dates, empty relations)
- Document assumptions and seed scale.

4) Establish a correctness baseline (mandatory)
For each crucial query:
- create a baseline correctness suite against the seeded sandbox DB:
  - snapshot expected results for fixed inputs (golden data)
  - include edge cases: NULLs, empty relations, duplicates, boundary dates, collation-sensitive strings
- store snapshots as machine-readable fixtures in `perf-lab/correctness/`
- correctness checks must be deterministic and runnable offline

5) Safety Gate A - Result equivalence check (mandatory for every variant)
For every query rewrite or alternative approach:
- verify the candidate returns the same logical result as baseline:
  - same rows (or same set) and same values for required columns
  - same ordering if the application depends on it (explicitly test ordering)
- if results differ:
  - mark the variant as `UNSAFE`
  - include a diff summary in the report
  - do not include it in recommended changes

6) Safety Gate B - Application-level contract check (mandatory when query is used by code)
- locate the call site(s) in PHP/JS
- infer the contract from usage:
  - required columns
  - assumptions about ordering
  - uniqueness assumptions
  - pagination assumptions (OFFSET/LIMIT stability)
  - NULL handling expectations
- add small "contract tests" that assert these assumptions against the sandbox DB
- if contract is unclear, mark the optimization as `UNKNOWN` and do not recommend it

7) Safety Gate C - Performance benchmark (only after A + B pass)
- benchmark only variants that passed correctness gates
- measure wall time and distribution:
  - warmup runs (ignore first N)
  - N iterations per query
  - report min / p50 / p95 / max
- capture EXPLAIN output when supported:
  - SQLite: `EXPLAIN QUERY PLAN`
  - MySQL/Postgres: `EXPLAIN (ANALYZE, BUFFERS)` when enabled

8) Safety Gate D - Index risk analysis
Adding an index can slow writes.
For each proposed index:
- estimate write overhead risk (qualitative)
- mark if the table is write-heavy
- recommend only if read wins justify it
- include rollback instructions

9) Generate improvement candidates (and test each one)
For each slow query, propose and validate:
- query rewrites:
  - remove `SELECT *`
  - reduce columns and rows early
  - replace subqueries with joins or vice versa (only if equivalent)
  - avoid functions on indexed columns
  - revise DISTINCT/GROUP BY patterns carefully
  - pre-aggregate or cache only if correctness contract allows it
- indexing ideas:
  - covering indexes
  - composite indexes with correct column order
  - partial indexes (Postgres) or generated columns when relevant
- application-level improvements (PHP/JS):
  - batch queries
  - memoize repeated lookups in request scope
  - avoid sequential awaits for independent reads (JS)
  - avoid building/parsing query strings in tight loops
Every improvement must have:
- a correctness proof (Gate A + B)
- a runnable benchmark
- clear rollback notes

10) Summarize ROI in seconds saved (only for SAFE changes)
Convert latency saved per request into time saved per day:
- `saved_sec_per_day = (baseline_ms - candidate_ms)/1000 * requests_per_day`

Provide scenarios:
- 1k, 10k, 100k requests/day (or based on logs if available)

Include confidence notes about representativeness of the sandbox.

## What to Look For
### SQL red flags
- N+1 queries (same query repeated per row)
- missing indexes on JOIN / WHERE / ORDER BY columns
- huge result sets where caller uses only a few fields
- GROUP BY / DISTINCT on large tables without supporting indexes
- correlated subqueries that scale badly
- LIKE patterns that prevent index use
- queries inside tight loops or called per-request

### Regression risks (correctness traps)
- DISTINCT vs GROUP BY changes result cardinality
- JOIN type changes (LEFT vs INNER)
- moving predicates between JOIN and WHERE
- NULL behavior differences
- time zone / collation differences
- pagination correctness (OFFSET/LIMIT stability)
- removing ORDER BY when caller depends on stable ordering

### PHP/JS red flags around DB usage
- repeated calls to the same data within a request
- fetching full rows when only 1-2 fields are used
- sequential awaits for independent DB reads (JS)
- building query strings repeatedly instead of parameter reuse

## Expected Output Format
### 1) Files created
- `perf-lab/README.md`
- `perf-lab/schema/` and `perf-lab/seed/`
- `perf-lab/correctness/` (golden fixtures + diff/check scripts)
- `perf-lab/queries/baseline.sql`
- `perf-lab/queries/variants/*.sql`
- `perf-lab/bench/run_bench.*` (runner script)
- `perf-lab/results/results.json`
- `perf-lab/reports/perf-report.md`

### 2) Report structure (`perf-lab/reports/perf-report.md`)
For each query:
- Identifier and where it is used (file paths, functions)
- Correctness
  - status: `SAFE` / `UNSAFE` / `UNKNOWN`
  - evidence: which fixtures, which contract tests
  - diff summary if UNSAFE
- Baseline performance metrics: p50, p95, max, iterations, seed scale
- Explain plan snippet
- Candidate A/B/C (SAFE only in the "recommended" section):
  - change description
  - SQL or index change
  - correctness evidence
  - benchmark results
  - delta (ms, %)
- ROI table (SAFE changes only):
  - 1k/day, 10k/day, 100k/day => seconds saved/day
- Risks:
  - correctness risks (if any)
  - migration risks
  - write amplification (indexes)
- Recommendation:
  - pick 1 best SAFE change (or say "no clear win")

### 3) Console summary (short)
- Top SAFE wins ranked by saved ms and confidence
- Total estimated seconds saved per day for scenario volumes

## Output Rules
- Never benchmark against the production database.
- Never recommend a change that fails result equivalence checks.
- Never claim a speedup without a runnable benchmark file.
- Separate "measured" from "hypothesized".
- Avoid changing business logic. If a rewrite may change results, flag it as UNSAFE.
- Use parameterized queries in benchmarks where relevant.
- Keep the lab self-contained and easy to delete.
- Do not commit secrets. Use example config.
- If you can gain speed only by changing semantics, present it as an optional trade-off:
  - clearly labeled,
  - not the default recommendation.

## Severity & Confidence
### Severity (performance impact)
- Critical: p95 slow on a hot endpoint or N+1 in a common path
- High: clear index/query plan problem with big wins
- Medium: moderate gains or lower traffic
- Low: micro-optimizations with tiny wins

### Confidence (performance measurement)
- High: realistic schema + realistic data + repeated runs + stable results
- Medium: schema matches but data approximated, still consistent win
- Low: limited realism, high variance, or DB engine mismatch

### Confidence (correctness)
- High: equivalence + contract tests + edge cases included
- Medium: equivalence + limited edge cases
- Low: equivalence only on small dataset or unclear contract
