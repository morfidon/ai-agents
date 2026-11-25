---
name: security-review
description: "Perform a defensive, read-only source review of a codebase the user is authorized to review. Use when the user explicitly asks for security hardening or wants to use spare tokens on a security review."
---

# Security Review

## Review one source boundary

1. Start from a code boundary the user can review, such as authentication, authorization, tenant isolation, secret handling, or untrusted input. Identify the documented protection requirement and where it is enforced.
2. Follow source code, tests, dependency manifests, and configuration for that boundary. Look for missing or inconsistent protections and explain the remediation. Keep the review static and avoid exposing secret values.
3. For each finding, report the affected file and line, expected control, source evidence, impact, confidence, and a proposed fix. Suggest a defensive test that asserts the required protection with ordinary fixtures.
4. Keep the work read-only and remediation-focused. Exclude exploit development, payloads, bypass attempts, vulnerability reproduction, and probing of running systems.
