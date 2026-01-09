PHASE 1 – Core Code Integrity

(Ensure code runs and behaves correctly at a basic level)

Syntax Bug – Static Lint AgentChecks syntax, unmatched symbols, and malformed code.

Logic Bug – Heuristic Reasoner AgentDetects wrong conditions, early returns, and inverted logic.

Runtime Bug – Crash Pattern AgentPredicts crashes: null deref, divide-by-zero, unguarded calls.

Bohrbug – Deterministic Failure AgentFinds logic paths that always fail, like missing defaults.

Heisenbug – Flake Risk AgentFlags non-determinism (time, random, race conditions).

Dead Code – Unused Things AgentFinds unreachable code, unused vars, or orphan assets.

PHASE 2 – Performance & Efficiency

(Check for unnecessary work and structural cost)

Performance Bug – Static Cost AgentScans loops, repeated queries, and missing caching.

Performance Regression – Benchmark Drift AgentCompares diffs to catch new loops, heavy joins, or cache removals.

Resource Usage – Infrastructure Cost AgentDetects over-provisioned, always-on, or idle expensive infra.

PHASE 3 – Architecture & Refactoring

(Ensure scalable, maintainable structure before integration)

Architecture Smell Radar AgentDetects god classes, layer leakage, anemic domains, util dumps.

Dependency Graph & Cycles AgentMaps imports, finds cycles, and bad dependency directions.

Layer Boundary & Ownership AgentChecks domain ↔ infra leaks, controllers using SQL, missing owners.

Coupling & Cohesion AgentFlags tight coupling, low cohesion, utility bloat.

Public API Surface Slimming AgentFinds unused exports, wide DTOs, and re-export barrels.

Transaction & Consistency AgentEnsures data writes are atomic, idempotent, and consistent.

Caching Strategy Coherence AgentValidates key schemes, TTLs, invalidations, SWR consistency.

Error-Handling Policy AgentStandardizes error envelopes, status codes, and logging.

Duplication & Abstraction AgentSpots copy-paste logic, repeated SQL, and missing abstractions.

Refactoring Opportunity Planner AgentAggregates signals from above to rank high-impact refactors.

PHASE 4 – Integration & Data Correctness

(Verify boundaries between systems and data accuracy)

Integration Bug – Interface Consistency AgentCompares API specs vs implementation (fields, types, codes).

Data Quality Bug – Validation AgentChecks schema alignment, nullability, and referential integrity.

Environment or Config Bug – Parity AgentDetects mismatched .env, Docker, and manifest versions.

Configuration Hygiene AgentFlags secrets in config, unused keys, debug flags, or wrong paths.

PHASE 5 – Dependency & Supply Chain

(Check provenance, safety, and license health)

Dependency Bug – Version & License AgentFinds version drift, deprecated libs, and license issues.

Dependency Supply-Chain Integrity AgentAdds trust checks: typosquats, unsigned packages, inactive maintainers.

PHASE 6 – Build & Delivery

(Validate that builds are reproducible and correct)

Build/CI Bug – Pipeline Rules AgentChecks pipeline steps, caching, triggers, and secret handling.

Build Artifact Drift AgentEnsures built outputs match manifests, versions, and checksums.

PHASE 7 – Security, Compliance & Observability

(Guardrails for safe, legal, and observable systems)

Security Bug – Static Vulnerability AgentDetects injections, hardcoded secrets, weak crypto.

Accessibility & Compliance AgentValidates WCAG, GDPR, cookie consent, data-deletion rights.

Observability Bug – Telemetry AgentChecks for proper logs, metrics, and tracing on key paths.

Feature Flag Bug – Toggle Consistency AgentEnsures flags exist, are consistent, and not inverted across envs.

PHASE 8 – Cross-Platform & Compatibility

(Guarantee portability across OS, runtimes, browsers)

Compatibility AgentDetects deprecated APIs, file-separator bugs, or missing fallbacks.

PHASE 9 – Testing, Documentation & UX

(Verify truth, clarity, and user experience)

Test Coverage & Integrity AgentLists untested or misleading test areas.

Documentation Drift – Docs vs Code AgentFinds differences between docs, comments, and code behavior.

UI or UX Bug – Interface Structure AgentChecks accessibility, overlap, z-index, labels, and color contrast.

Localization Bug – i18n/L10n AgentDetects hardcoded strings, missing locales, or format errors.

PHASE 10 – Aggregation & Reporting

(Summarize all results for decision-making)

Audit Orchestrator AgentCollects all findings, deduplicates overlaps, ranks by severity and confidence.
