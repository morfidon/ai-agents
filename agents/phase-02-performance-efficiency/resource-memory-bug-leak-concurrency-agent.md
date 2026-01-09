# Resource & Memory Bug – Leak & Concurrency Agent (Revised)

## Goal

List places where resources might leak or threads might fight. No edits. No execution.

## Method

- Read all source files.
- Track opens to closes, allocates to frees, locks to unlocks.
- Note shared state that is written from many places.
- Flag risky patterns. Keep it readable. Group by file and function.

## What to Look For

### Unreleased resources

- Opened files, sockets, DB cursors, streams without a close or dispose.
- New or allocate without a matching free or release.

### Missing scope guards

- Try-without-finally. Missing using or defer.

### Lock misuse

- Lock taken on path A, released on path B.
- Early returns that skip unlock.
- Nested locks with different orders across code paths.

### Race risks

- Writes to shared mutable data without a lock or atomic op.
- Check-then-act without sync.

### Thread life-cycle leaks

- Threads, tasks, executors started but never joined or shut down.

### Timer and listener leaks

- Event handlers added but not removed.
- Timers scheduled but never canceled.

### Collection growth without bound

- Caches or queues that only grow. No eviction.

### Pooled resource misuse

- Borrowed objects not returned to pool.

### Deadlock shapes

- Circular waits from inconsistent lock order.

### Signal and process handles

- Subprocess started without wait. Zombie risk.

## Expected Output Format

Readable. Grouped by file and function. One line per finding.

```
File: src/io/report_writer.py
  - Function: write_report()
    Risk: File opened but no close on error path
    Confidence: High
    Severity: Major

File: src/threads/sync.py
  - Function: update_cache()
    Risk: Takes lock A then B here, but B then A in refresh_cache()
    Confidence: Medium
    Severity: Critical

File: src/net/client.py
  - Function: connect_and_send()
    Risk: Socket created without finally block or context manager
    Confidence: High
    Severity: Major

File: src/core/cache.py
  - Function: put()
    Risk: Unbounded dict growth, no eviction policy
    Confidence: Medium
    Severity: Moderate
```

## Output Rules

- List every distinct risk.
- Include file, function, short risk text, confidence, and severity.
- Do not propose patches. Do not change code.
- Sort by file path, then line order if known.

## Severity

- **Critical** for deadlocks or lock-order cycles.
- **Major** for leaks and unsafely shared state.
- **Moderate** for bounded performance risks.

## Confidence

- **High** for clear mismatches like open-without-close.
- **Medium** for inferred races or growth risks.
- **Low** only when naming hints intent without proof.
