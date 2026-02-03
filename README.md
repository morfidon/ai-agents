# AI Agents Repository

A comprehensive collection of specialized AI agents designed to systematically analyze, audit, and improve software projects across multiple dimensions of quality, security, and maintainability.

## Overview

This repository contains **50+ specialized agents** organized into **10 phases** that cover the entire software development lifecycle. Each agent is designed to identify specific issues, risks, and improvement opportunities without modifying code - they provide detailed analysis and reporting to help developers make informed decisions.

## How It Works

Each agent follows a consistent methodology:
- **Goal**: Clear objective of what the agent analyzes
- **Method**: Systematic approach to examining code and artifacts
- **What to Look For**: Specific patterns and issues to identify
- **Expected Output Format**: Structured, readable findings
- **Output Rules**: Guidelines for reporting results
- **Severity & Confidence**: Framework for prioritizing findings

## Phases & Agents

### [Phase 1 – Core Code Integrity](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity)
*(Ensure code runs and behaves correctly at a basic level)*

- **[Syntax Bug – Static Lint Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/1.syntax-bug-static-lint-agent.md)** - Checks syntax, unmatched symbols, and malformed code
- **[Logic Bug – Heuristic Reasoner Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/2.logic-bug-heuristic-reasoner-agent.md)** - Detects wrong conditions, early returns, and inverted logic
- **[Runtime Bug – Crash Pattern Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/3.runtime-bug-crash-pattern-agent.md)** - Predicts crashes: null deref, divide-by-zero, unguarded calls
- **[Integration Bug – Contract Diff Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/4.integration-bug-contract-diff-agent.md)** - Compares API specs vs implementation (fields, types, codes)
- **[Bohrbug – Deterministic Failure Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/5.bohrbug-deterministic-failure-agent.md)** - Finds logic paths that always fail, like missing defaults
- **[Heisenbug – Flake Risk Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/6.heisenbug-flake-risk-agent.md)** - Flags non-determinism (time, random, race conditions)
- **[Dead Code – Unused Things Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-01-core-code-integrity/7.dead-code-unused-things-agent.md)** - Finds unreachable code, unused vars, or orphan assets

### [Phase 2 – Performance & Efficiency](https://github.com/morfidon/ai-agents/blob/main/agents/phase-02-performance-efficiency)
*(Check for unnecessary work and structural cost)*

- **[Performance Bug – Static Cost Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-02-performance-efficiency/performance-bug-static-cost-agent.md)** - Scans loops, repeated queries, and missing caching
- **[Performance Regression – Benchmark Drift Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-02-performance-efficiency/performance-regression-benchmark-drift-agent.md)** - Compares diffs to catch new loops, heavy joins, or cache removals
- **[Resource Memory Bug – Leak Concurrency Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-02-performance-efficiency/resource-memory-bug-leak-concurrency-agent.md)** - Detects memory leaks and concurrency issues
- **[Resource Usage – Infrastructure Cost Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-02-performance-efficiency/resource-usage-infrastructure-cost-agent.md)** - Detects over-provisioned, always-on, or idle expensive infra

### [Phase 3 – Architecture & Refactoring](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring)
*(Ensure scalable, maintainable structure before integration)*

- **[Architecture Smell Radar Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/1.architecture-smell-radar-agent.md)** - Detects god classes, layer leakage, anemic domains, util dumps
- **[Dependency Graph & Cycles Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/2.dependency-graph-cycles-agent.md)** - Maps imports, finds cycles, and bad dependency directions
- **[Layer Boundary & Ownership Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/3.layer-boundary-ownership-agent.md)** - Checks domain ↔ infra leaks, controllers using SQL, missing owners
- **[Coupling & Cohesion Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/4.coupling-cohesion-agent.md)** - Flags tight coupling, low cohesion, utility bloat
- **[Public API Surface Slimming Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/5.public-api-surface-slimming-agent.md)** - Finds unused exports, wide DTOs, and re-export barrels
- **[Transaction & Consistency Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/6.transaction-consistency-agent.md)** - Ensures data writes are atomic, idempotent, and consistent
- **[Caching Strategy Coherence Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/7.caching-straegy-coherence-agent.md)** - Validates key schemes, TTLs, invalidations, SWR consistency
- **[Error-Handling Policy Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/8.error-handling-policy-agent.md)** - Standardizes error envelopes, status codes, and logging
- **[Duplication & Abstraction Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/9.duplication-abstraction-agent.md)** - Spots copy-paste logic, repeated SQL, and missing abstractions
- **[Refactoring Opportunity Planner Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/10.refactoring-oppurtunity-planner-agent.md)** - Aggregates signals from above to rank high-impact refactors
- **[Proactive Improvement Ideas Agent (Optional)](https://github.com/morfidon/ai-agents/blob/main/agents/phase-025-architecture-refactoring/11.proactive-improvement-ideas-agent-optional.md)** - Suggests architectural improvements based on patterns

### [Phase 3 – Integration & Data Correctness](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness)
*(Verify boundaries between systems and data accuracy)*

- **[Integration Bug – Interface Consistency Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness/1.integration-bug-interface-consistency-agent.md)** - Compares API specs vs implementation (fields, types, codes)
- **[Data Quality Bug – Validation Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness/2.data-quality-bug-agent-validation-agent.md)** - Checks schema alignment, nullability, and referential integrity
- **[Environment or Config Bug – Parity Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness/3.environment-config-bug-parity-agent.md)** - Detects mismatched .env, Docker, and manifest versions
- **[Configuration Hygiene Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness/4.configuration-hygiene-agent.md)** - Flags secrets in config, unused keys, debug flags, or wrong paths
- **[Hardcoded Config Value Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-03-integration-data-correctness/5.hardcoded-config-value-agent.md)** - Detects hardcoded configuration values in code

### [Phase 4 – Dependency & Supply Chain](https://github.com/morfidon/ai-agents/blob/main/agents/phase-04-dependency-supply-chain)
*(Check provenance, safety, and license health)*

- **[Dependency Bug – Version & License Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-04-dependency-supply-chain/1.dependency-bug-version-license-agent.md)** - Finds version drift, deprecated libs, and license issues
- **[Dependency Supply-Chain Integrity Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-04-dependency-supply-chain/2.dependency-supply-chain-integrity-agent.md)** - Adds trust checks: typosquats, unsigned packages, inactive maintainers

### [Phase 5 – Build & Delivery](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery)
*(Validate that builds are reproducible and correct)*

- **[IaC Policy Drift Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery/1.iac-policy-drift-agent.md)** - Detects infrastructure as code configuration drift
- **[Build CI Bug – Pipeline Rules Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery/2.build-ci-bug-pipeline-rules-agent.md)** - Checks pipeline steps, caching, triggers, and secret handling
- **[Migration Safety Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery/3.migration-safety-agent.md)** - Validates database migration safety and rollback procedures
- **[Build Artifact Drift Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery/4.build-artifact-drift-agent.md)** - Ensures built outputs match manifests, versions, and checksums
- **[Backup & Restore Readiness Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-05-build-delivery/5.backup-restore-readiness-agent.md)** - Validates backup strategies and restore procedures

### [Phase 6 – Security, Privacy & Compliance](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance)
*(Guardrails for safe, legal, and observable systems)*

- **[Security Bug – Static Vulnerability Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/1.security-bug-static-vulnerability-agent.md)** - Detects injections, hardcoded secrets, weak crypto
- **[Accessibility & Compliance Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/2.accessibility-compliance-agent.md)** - Validates WCAG, GDPR, cookie consent, data-deletion rights
- **[Data Lineage & PII Mapping Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/3.data-lineage-pii-mapping-agent.md)** - Maps data flow and identifies PII handling gaps
- **[Observability Bug – Telemetry Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/4.observability-bug-telemetry-agent.md)** - Checks for proper logs, metrics, and tracing on key paths
- **[Feature Flag Bug – Toggle Consistency Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/5.feature-flag-bug-toggle-consistency-agent.md)** - Ensures flags exist, are consistent, and not inverted across envs
- **[LLM / Prompt Safety Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/6.llm-prompt-safety-agent.md)** - Identifies prompt injection risks and AI safety issues
- **[Multi-Tenancy Isolation Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/7.multi-tenancy-isolation-agent.md)** - Validates tenant data isolation and access controls
- **[Rate Limit & Abuse Guard Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-06-security-privacy-compliance/7.rate-limit-abuse-guard-agent.md)** - Checks for proper rate limiting and abuse prevention

### [Phase 7 – Cross-Platform Compatibility](https://github.com/morfidon/ai-agents/blob/main/agents/phase-07-cross-platform-compatibility)
*(Guarantee portability across OS, runtimes, browsers)*

- **[Compatibility Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-07-cross-platform-compatibility/1.compatibility-agent.md)** - Detects deprecated APIs, file-separator bugs, or missing fallbacks
- **[Mobile App Permissions Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-07-cross-platform-compatibility/2.mobile-app-permissions-agent.md)** - Analyzes mobile app permissions and privacy compliance

### [Phase 8 – Testing, Documentation & Maintenance](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance)
*(Verify truth, clarity, and user experience)*

- **[Test Coverage & Integrity Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/1.test-coverage-integrity-agent.md)** - Lists untested or misleading test areas
- **[Documentation Drift – Docs vs Code Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/2.documentation-drift-docs-vs-code-agent.md)** - Finds differences between docs, comments, and code behavior
- **[API Deprecation Lifecycle Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/3.api-deprecation-lifecycle-agent.md)** - Ensures APIs evolve safely with proper deprecation
- **[UI/UX Bug – Interface Structure Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/3.ui-ux-bug-interface-structure-agent.md)** - Checks accessibility, overlap, z-index, labels, and color contrast
- **[Localization Bug – i18n/L10n Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/4.localization-bug-i18n-l10n-agent.md)** - Detects hardcoded strings, missing locales, or format errors
- **[Critical Unit Test Prioritization Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/5.critical-unit-test-prioritization-agent.md)** - Ranks missing unit tests by criticality with impact rationale
- **[Mobile UI/UX Critical Changes Agent](https://github.com/morfidon/ai-agents/blob/main/agents/phase-08-testing-documentation-maintenance/6.mobile-ui-ux-critical-changes-agent.md)** - Prioritizes critical mobile UI/UX improvements with rationale and creative suggestions

### [Phase 9 – Aggregation & Reporting](https://github.com/morfidon/ai-agents/blob/main/agents/different)
*(Summarize all results for decision-making)*

- **[Audit Orchestrator Agent](https://github.com/morfidon/ai-agents/blob/main/agents/different/1.commit-agent.md)** - Collects all findings, deduplicates overlaps, ranks by severity and confidence

## Usage

Each agent is designed to be run independently or as part of a comprehensive audit. They follow a **read-only** approach - they analyze code and artifacts without making changes, providing detailed reports that developers can use to prioritize improvements.

### Key Principles

- **No Code Modification**: Agents only read and analyze, never modify
- **Structured Output**: Consistent, readable format for all findings
- **Severity & Confidence**: Clear prioritization framework
- **Comprehensive Coverage**: From syntax to security to user experience
- **Systematic Approach**: Each agent has clear methodology and scope

## Target Audience

- **Development Teams**: Looking to systematically improve code quality
- **DevOps Engineers**: Validating build, deployment, and infrastructure practices
- **Security Teams**: Identifying vulnerabilities and compliance gaps
- **QA Teams**: Enhancing test coverage and documentation accuracy
- **Architects**: Ensuring maintainable, scalable system design

## Contributing

This repository is designed as a comprehensive toolkit for software quality analysis. Each agent is self-contained with clear documentation and can be extended or customized for specific project needs.

---

*This repository provides a systematic approach to software quality assurance, helping teams identify and address issues across the entire development lifecycle.*
