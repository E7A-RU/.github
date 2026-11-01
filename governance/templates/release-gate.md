# E7A Release / Launch Gate — evidence template

> **TEMPLATE / NOT A DEPLOYMENT AUTHORIZATION.** Use together with [Product Lifecycle checklists](../product-lifecycle-checklists.md), [Infra](../infra-operations.md), [approved Governance](../README.md). This document is product-local evidence, not the canonical authorization record by itself.

See also the [applicable Kit roadmap](../kit-implementation-roadmaps.md) and [backend DB/security acceptance](../backend-reference-architecture.md) when stateful. A browser-only app is **not** required to invent a database/worker.

## Product and immutable release

- Product/repository: `<id and owner/repo>`
- Product Kit / governance SHA: `<kit>/<full-approved-sha>`
- Release candidate full source SHA: `<40-hex>`
- Build provenance / immutable image/package digest: `<sha256 or n/a>`
- Target: `production` (optional `staging` if risk-required); environment ID and deployment owner:
- Change classification: `R0 / R1 / R2 / R3 / R4`
- User/customer/tenant impact:
- Standing authorization ID / approved risk allowlist (or per-release approval ID), window / expiry:
- Compared-to current production SHA/artifact:

## Technical checks (fill actual evidence)

- [ ] Exact-SHA local/self-hosted install/build/typecheck/lint/unit/integration/negative tests — logs:
- [ ] Supply-chain/locked dependencies, secret scan and artifact integrity — evidence:
- [ ] Compatibility, database migration/up/down and backwards constraints — evidence:
- [ ] Browser/mobile/accessibility or worker/idempotence/connector provider tests as applicable:
- [ ] Isolated preflight/QA smoke/health and thresholds; risk-triggered staging evidence **only if required**:
- [ ] Security & privacy: scoped permissions, key handling, audit/retention, tenant negative tests:
- [ ] Off-host backup, restore drill and rollback/forward-fix plan for stateful services:
- [ ] Existing host/other product blast radius and resource limits confirmed:
- [ ] Support/operations notification, monitoring, incident and recovery contact confirmed:

### Actual check states (never coerce UNKNOWN to PASS)

| Gate | PASS / FAIL / SKIPPED / UNKNOWN | Evidence ref / exact SHA | Owner |
| --- | --- | --- | --- |
| Typecheck/build | UNKNOWN | | |
| Functional/negative/security | UNKNOWN | | |
| Privacy/tenant | UNKNOWN | | |
| Staging (optional; NOT_REQUIRED with documented reason) | UNKNOWN | | |
| Backup/restore if required | UNKNOWN | | |
| Infra risk assessment | UNKNOWN | | |

## Authorization and rollout

- Laboratory QA evidence ID / artifact digest:
- Independent technical reviewer if applicable:
- Temporary owner-only critical approval exception ID/expiry and compensating controls:
- **Production authorization:** `NOT_GRANTED` until Infra verifies either (a) active product-specific standing owner authorization and this merge's protected-main/allowlist/QA/digest eligibility, or (b) explicit per-release owner approval bound to SHA/digest/target/validity.
- Infra independent authorization validation: `NOT_DONE`.
- Apply plan/dry-run reference:
- Health and deployment verification timestamps:
- Rollback criteria, monitored rollback artifact, access to recovery path:
- State: `PROPOSED / TESTED / RELEASE ELIGIBLE / RUNNING VERIFIED / ROLLED BACK / RECOVERY REQUIRED / FAILED / UNKNOWN`; `STAGING VERIFIED` and `INDIVIDUAL OWNER APPROVED` only when required.

**Hard stop:** an open/merged PR or this checklist **alone** cannot grant production authorization. The only merge-triggered path is an **already activated, scoped standing authorization** with protected-main and exact-SHA passing evidence. Otherwise per-release approval is required. No GitHub-hosted Actions CI. See [release/deployment policy](../release-deployment-policy.md).
