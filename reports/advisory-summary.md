# Dependency Audit Report

## Scope

This report records the security advisories currently published for the npm ecosystem at **critical severity**, gathered as part of the dependency-security audit of our Node service. The effort is tracked in issue #1 (`Dependency security audit tracker`), with sub-tasks in #2 (`Collect advisory data`), #3 (`Review affected packages`), and #4 (`Publish audit report`).

## Critical npm advisories

The advisory lookup for the npm ecosystem at critical severity returned **2 advisories**:

| # | GHSA ID | Affected package(s) | CVE |
| - | ------- | -------------------- | --- |
| 1 | GHSA-qx66-fv34-fjm8 | @tanstack/start-server-core, @tanstack/react-start, @tanstack/solid-start, @tanstack/vue-start | CVE-2026-102989 |
| 2 | GHSA-x34j-47hf-4xg7 | tinacms, @tinacms/app | CVE-2026-108261 |

## Advisory details

### 1. GHSA-qx66-fv34-fjm8 — CVE-2026-102989

- **Severity:** Critical (CVSS 9.3 — CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N)
- **Summary:** TanStack Router: Unauthenticated reflected XSS in TanStack Start server-function responses
- **Affected packages (npm):** `@tanstack/start-server-core`, `@tanstack/react-start`, `@tanstack/solid-start`, `@tanstack/vue-start` — versions from `1.143.12` up to, but not including, each package's patched version
- **First patched versions:** `@tanstack/start-server-core@1.169.39`, `@tanstack/react-start@1.168.60`, `@tanstack/solid-start@1.168.57`, `@tanstack/vue-start@1.168.56`
- **CWE:** CWE-79 (Cross-site Scripting)
- **CVE:** CVE-2026-102989

An unauthenticated attacker can craft a URL to a server function in an affected TanStack Start app. If someone opens the link, the app can return attacker-controlled HTML from its own origin and run the attacker's JavaScript there, accessing same-origin data and making requests with the visitor's authority. The fix limits client-supplied server-function input to public fields and validates responses at the server boundary.

### 2. GHSA-x34j-47hf-4xg7 — CVE-2026-108261

- **Severity:** Critical (CVSS 9.3 — CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N)
- **Summary:** TinaCMS admin preview iframe loads an attacker-controlled origin from the URL fragment
- **Affected packages (npm):** `tinacms`, `@tinacms/app`
- **CWEs:** CWE-346 (Origin Validation Error), CWE-441 (Unintended Proxy or Intermediary), CWE-601 (Open Redirect)
- **CVE:** CVE-2026-108261

The TinaCMS admin builds its preview iframe source from the `/~/*` hash-router splat without checking that the value stays same-origin. A fragment with a doubled slash (for example `#/~//attacker.example/p`) becomes a protocol-relative URL to an external site, and the admin frames it. Because that same unvalidated string derives the only trust anchor for the admin-to-preview `postMessage` channel, the attacker's frame is treated as trusted: one link, opened by a logged-in editor, gives an unauthenticated remote attacker arbitrary read and write access to the site's content API as that editor.

## Next steps

1. Cross-reference the affected packages above against our Node service's dependency tree (#3, `Review affected packages`).
2. Upgrade any matching dependency to its first patched version; where an immediate upgrade is not possible, apply the mitigations documented in each advisory.
3. Finalize and publish this report (#4, `Publish audit report`).
