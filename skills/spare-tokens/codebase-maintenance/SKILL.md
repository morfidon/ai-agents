---
name: codebase-maintenance
description: "Find small, evidence-backed refactor opportunities when spare agent capacity is available or a source area has become difficult to change safely."
---

# Codebase Maintenance

## Find a change worth making

1. Pick one module or feature using a recent edit, repeated logic, or a fragile boundary. Inspect its contracts, direct callers, and nearby tests.
2. Look for duplicated business rules that can drift, abstractions that hide side effects, stale paths, and coupling that forces unrelated edits. Show the consequence of each candidate.
3. Propose up to three narrow changes. For each, identify the code, the behavior to preserve, who depends on it, the expected benefit, and a verification step.
4. Prefer one small behavior-preserving change with a clear reason. Leave files untouched and note where tests or implementation evidence are missing.
