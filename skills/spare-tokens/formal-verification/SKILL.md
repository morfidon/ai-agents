---
name: formal-verification
description: "Specify and check one critical correctness property with a bounded model. Use for business invariants, event ordering, idempotency, or lifecycle rules that can be represented with explicit states and transitions."
---

# Formal Verification

## Specify and check one property

1. Choose one property with concrete impact and cite the code and tests it comes from. Name the initial states, transitions, actors, and environmental assumptions.
2. Define a finite model and record its limits, such as actor count, retry depth, queue capacity, or ordering assumptions. Check that the property can actually fail in the model by examining a deliberately weakened variant.
3. If an appropriate checker is already available, run it on a self-contained model using temporary files outside the project. For small domains, enumerate reachable states. If exhaustive checking is unavailable, give the specification and manual analysis and explain which check remains open.
4. Report the tool or method, explored bounds, and result. For a failing check, give the shortest counterexample you observed and map the transitions back to the source. For a passing check, state the guarantee within the model and identify what remains outside it.
