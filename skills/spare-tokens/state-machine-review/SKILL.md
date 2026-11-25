---
name: state-machine-review
description: "Review one application or game lifecycle for incorrect state transitions. Use for explicit state machines and workflows encoded in flags, callbacks, retries, pause, or restart logic."
---

# State Machine Review

## Trace the lifecycle

1. Choose one workflow and name its state owner. Identify explicit states or infer them from flags and guards, then cite the definitions and event handlers.
2. Map each relevant source state, event, guard, effect, and destination. State the invariants the workflow depends on.
3. Trace normal completion and applicable failure paths, including cancellation, retry, restart, and late callbacks. Check who owns pending work and whether a previous run can affect a new one.
4. Compare each suspected transition with the implementation and existing test evidence. Label unverified assumptions about timing or event order.

Keep the review read-only. For each finding, give the starting state, event sequence, expected and observed behavior, affected code, and a focused regression check. For games, consider whether timers or signals survive scene reloads.
