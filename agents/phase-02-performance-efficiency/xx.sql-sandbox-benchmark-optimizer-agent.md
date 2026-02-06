# SQL Sandbox Benchmark & Optimizer Agent

## Goal
Create a safe, repeatable performance lab that:
- benchmarks crucial SQL queries on a temporary database (no impact on prod),
- proposes alternative query strategies (and indexes),
- validates each proposal with runnable benchmark files,
- summarizes expected time saved (seconds) based on realistic request volumes.

This agent may also spot similar "hot paths" in PHP/JS that call those queries, but it must validate assumptions by writing and running microbenchmarks in a dedicated folder.

## Method
1) Discover "crucial queries"
- Identify candidate SQL queries by scanning:
  - DB access layers, repositories, query builders, raw SQL strings.
  - endpoints or jobs that run frequently or are latency sensitive.
  - queries inside loops, N+1 patterns, repeated lookups.
- Create a short list (top 5-15) based on likely frequency and cost.

2) Create an isolated benchmark folder
- Create a new folder (default: `perf-lab/`) containing:
  - `README.md` (how to run, what it measures)
  - `config.example.*` (no secrets)
  - `seed/` (seed scripts and/or fixtures)
  - `queries/` (baseline SQL and variants)
  - `results/` (machine-readable output, JSON)
  - `reports/` (human-readable summary, Markdown)

3) Build a temporary database that mimics production shape
- Use a throwaway DB so prod is never touched.
- Default choice: SQLite (fast to spin up, good for regression style checks).
- If the repo uses MySQL/Postgres features, add an optional Docker setup:
  - `docker-compose.perf.yml` that runs a temporary DB and loads schema/seed.

4) Recreate schema and representative data
- Reuse migrations or schema definitions when possible.
- Seed enough rows to make plans realistic:
  - include skew (popular users, common statuses, etc.)
  - include edge cases (nulls, long strings, missing relations)
- Document assumptions and seed scale.

5) Benchmark baseline queries
- Measure wall time and distribution:
  - warmup runs (ignore first N)
  - N iterations per query
  - report min / p50 / p95 / max
- Capture EXPLAIN output when supported:
  - SQLite: `EXPLAIN QUERY PLAN`
  - MySQL/Postgres: `EXPLAIN (ANALYZE, BUFFERS)` when enabled

6) Generate improvement candidates and test them
For each slow query, propose and test:
- query rewrites:
  - remove SELECT *
  - reduce columns and rows early
  - replace subqueries with joins or vice versa
  - avoid functions on indexed columns
  - change DISTINCT/GROUP BY patterns
  - pre-aggregate, materialize, or cache if appropriate
- indexing ideas:
  - covering indexes
  - composite indexes with correct column order
  - partial indexes (Postgres) or generated columns when relevant
- application-level improvements (PHP/JS):
  - batch queries
  - memoize repeated lookups in request scope
  - avoid re-parsing or rebuilding query strings in loops
Each improvement must have a runnable benchmark that proves it.

7) Summarize ROI in seconds saved
- Convert latency saved per request into time saved per day:
  - `saved_sec_per_day = (baseline_ms - candidate_ms)/1000 * requests_per_day`
- Provide scenarios:
  - 1k, 10k, 100k requests/day (or based on logs if available)
- Include confidence notes about representativeness of the sandbox.

## What to Look For
### SQL red flags
- N+1 queries (same query repeated per row)
- missing indexes on JOIN / WHERE / ORDER BY columns
- huge result sets where caller uses only a few fields
- GROUP BY / DISTINCT on large tables without supporting indexes
- correlated subqueries that scale badly
- LIKE patterns that prevent index use
- queries inside tight loops or called per-request

### PHP/JS red flags around DB usage
- repeated calls to the same data within a request
- fetching full rows when only 1-2 fields are used
- sequential awaits for independent DB reads (JS)
- building query strings repeatedly instead of parameter reuse

## Expected Output Format
### 1) Files created
- `perf-lab/README.md`
- `perf-lab/schema/` and `perf-lab/seed/`
- `perf-lab/queries/baseline.sql`
- `perf-lab/queries/variants/*.sql`
- `perf-lab/run_bench.*` (runner script)
- `perf-lab/results/results.json`
- `perf-lab/reports/perf-report.md`

### 2) Report structure (`perf-lab/reports/perf-report.md`)
For each query:
- Identifier and where it is used (file paths, functions)
- Baseline metrics: p50, p95, max, iterations, seed scale
- Explain plan snippet
- Candidate A/B/C:
  - change description
  - SQL or index change
  - benchmark results
  - delta (ms, %)
- ROI table:
  - 1k/day, 10k/day, 100k/day => seconds saved/day
- Risks:
  - correctness risks
  - migration risks
  - write amplification (indexes)
- Recommendation:
  - pick 1 best change (or say "no clear win")

### 3) Console summary (short)
- Top wins ranked by saved ms and confidence
- Total estimated seconds saved per day for scenario volumes

## Output Rules
- Never benchmark against the production database.
- Never claim a speedup without a runnable benchmark file.
- Separate "measured" from "hypothesized".
- Avoid changing business logic. If a rewrite may change results, flag it.
- Use parameterized queries in benchmarks where relevant.
- Keep the lab self-contained and easy to delete.
- Do not commit secrets. Use example config.

## Severity & Confidence
### Severity (performance impact)
- Critical: p95 slow on a hot endpoint or N+1 in a common path
- High: clear index/query plan problem with big wins
- Medium: moderate gains or lower traffic
- Low: micro-optimizations with tiny wins

### Confidence (truthfulness of the win)
- High: sandbox mirrors schema + realistic data + repeated runs + stable results
- Medium: schema matches but data is approximated, still consistent win
- Low: limited data realism, high variance, or DB engine mismatch
