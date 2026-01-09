# Performance Bug - Static Cost Agent (Revised)

## Goal

List code that looks slow. No edits. No execution.

## Method

- Read all source files.
- Estimate cost from structure.
- Flag classic slow patterns.
- Group results by file and function.

## What to Look For

### Nested loops on big data

- Double or triple loops over lists, maps, or DB rows.
- Loop inside loop with lookups.

### N+1 queries

- Query inside a loop.
- ORM calls that fetch one row per item.

### Hot string work

- String concat in loops.
- Regex in tight loops.

### Unneeded allocations

- New objects each iteration.
- Copying large arrays or collections.

### Missing caches

- Recomputing pure results.
- Re-reading the same config or file.

### Heavy conversions

- JSON encode-decode in loops.
- Date parsing in hot paths.

### Unbounded sorts and filters

- Sort everything, then slice.
- Filter after expand.

### Wide selects

- SELECT * when a few fields are used.

### Eager work on cold paths

- Build full objects when only IDs are needed.

### Sync I-O in loops

- Blocking disk or network inside iteration.

### Inefficient data structures

- Linear search where a set or map fits.

### Redundant serialization

- Convert to JSON, then back, repeatedly.

## Expected Output Format

Readable. One line per finding. Grouped by file and function.

```
File: src/reports/generator.py
  - Function: build_report()
    Hotspot: Query executed inside for-loop over 5k items (N+1 risk)
    Reason: One DB call per item
    Confidence: High
    Severity: Major

File: src/core/strings.ts
  - Function: joinNames()
    Hotspot: String concatenation in loop
    Reason: Builds many temporary strings
    Confidence: High
    Severity: Moderate

File: services/orders/service.java
  - Method: computeTotals()
    Hotspot: Nested loops over orders x items
    Reason: O(n^2) growth
    Confidence: Medium
    Severity: Major

File: src/utils/date.js
  - Function: normalize()
    Hotspot: Date parse on every iteration
    Reason: Expensive conversion in hot path
    Confidence: Medium
    Severity: Moderate

File: api/users/repo.sql
  - Query: fetch_users
    Hotspot: SELECT * used but only id,email read
    Reason: Extra I-O and network
    Confidence: High
    Severity: Moderate
```

## Output Rules

- List every hotspot you find.
- Include file, function or query, hotspot name, short reason, confidence, and severity.
- Do not propose code changes. Do not measure time.
- Sort by file path, then by line number if known.
- Prefer concrete phrases seen in code.

## Severity

- **Major** for N+1, O(n^2+) on big data, sync I-O in loops.
- **Moderate** for string churn, extra fields, repeated parsing.

## Confidence

- **High** when the pattern is explicit in the code.
- **Medium** when intent is inferred from names or comments.
- **Low** only if data size is unclear.
