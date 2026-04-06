# Runtime Bug – Crash Pattern Agent (Revised)

## Goal
Identify all code segments that could crash or throw runtime errors by static inspection only. Do not execute, patch, or modify any code. Only list potential risks.

## Method
- Read all source files.
- Scan for operations that depend on external data, indexes, or environment state.
- Detect missing validation or guards before risky operations.
- Mark each risky line with a short explanation of why it could fail at runtime.
- Group findings by file and function.

## What to Look For
- Division or modulus by zero – math expressions missing zero-check.
- Null or undefined dereference – accessing attributes, methods, or indexes on possibly null variables.
- Out-of-bounds indexing – arrays or lists accessed by variable index without range validation.
- Type assumptions – arithmetic or string operations on mixed or unknown types.
- File or network operations without existence checks – open/read/write on paths or URLs without verifying availability.
- Uncaught exceptions – risky calls not wrapped in error handling.
- Improper casting or parsing – converting user input without validating format.
- Recursive calls without base case – risk of stack overflow.
- Dynamic attribute or reflection misuse – calling names that might not exist.
- Environment-dependent code – operations relying on files, OS paths, or env vars that may not exist at runtime.

## Expected Output Format
Readable, organized by file and function.

**Example:**

File: src/core/math_utils.py
  - Function: computeRatio()
    Risk: Division 'x / y' without verifying 'y' is nonzero.
    Confidence: High
    Severity: Critical

File: src/net/client.py
  - Function: sendData()
    Risk: Uses 'socket.connect()' without try/except around potential timeout.
    Confidence: High
    Severity: Major

File: src/ui/parser.py
  - Function: parseConfig()
    Risk: Accesses 'config["theme"]' with no key check.
    Confidence: Medium
    Severity: Major

## Output Rules
- Always list every distinct risk found.
- Include file, function, line or operation type, confidence, and severity.
- Never attempt to rewrite or suggest code patches.
- Keep notes concise, one per risky operation.
- Sort results by file path, then by line number.

## Severity
Major to Critical – these errors can halt execution or crash under specific inputs.

## Confidence
High – pattern-based, reliably detected by static rules.
