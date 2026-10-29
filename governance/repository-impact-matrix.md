# Repository Impact Matrix and dependency register

**Status: PROPOSED migration plan, no consumer repo mutation.** Important: historical queue below is superseded in sequencing by [final owner baseline](final-architecture-baseline-2026-10-27.md): Identity, Platform, Audit and bounded Billing enter F1; operational status remains unverified. Canonical source is [governance](README.md); all consumers adopt a **released immutable commit SHA/tag**, not this Draft branch's moving head.

## Components and ownership

| Component / repo | Responsibility and data ownership | Dependencies | Verified disposition / next action |
| --- | --- | --- | --- |
| [.github](https://github.com/E7A-RU/.github) | Governance, brand policy, architecture decisions; no runtime data | none | Documentation exists in Draft PR #1; approval and immutable revision pending |
| [core](https://github.com/E7A-RU/core) | `@e7a-ru/ui`, `@e7a-ru/server`, types, optional Starter; **no** Identity DB | Published package dependencies for consumers, not shared runtime | Source/docs exist; each release must be verified separately |
| [website](https://github.com/E7A-RU/website) | Public marketing, catalog, demo entry and lead intake; its own records if any | optional Core UI, own hosting | Consumer governance-pin proposal; verify production separately |
| [reader](https://github.com/E7A-RU/reader) | Browser local-first reading/user files; no mandatory server DB | optional Core UI; browser runtime | Keep anonymous offline/local behavior; acceptance test |
| [bot](https://github.com/E7A-RU/bot) | Bot domain logic, tenant ACL, messaging scenarios and owned persistence | own API/DB/workers; optional Core and external providers | No assumed migration to Platform; compatibility assessment |
| [laboratory](https://github.com/E7A-RU/laboratory) | Product factory specs, runner evidence, approvals, Developer/Partner/Staff DX | Core Starter + future Identity optional by phase; Infra sandbox | Capability is conceptual until runtime evidence; create pinned standard consumer check proposal |
| [infra](https://github.com/E7A-RU/infra) | Compute/network/TLS/secrets/storage provisioning and controlled deployment; not product business data | product manifests; owner approvals | Protect KS54/Lobachevsky; no changes outside separate Infra PR |
| Identity (future repo not confirmed) | Person/org identities, assurance, SSO, access policies and own DB | Mature IdP + own Person/Claims/Policy, Infra runtime when built | APPROVED EARLY F1 SCOPE / implementation unverified; not mandatory to Reader |
| Platform (future repo not confirmed) | Delivery, trusted Jobs, Connectors, Results and own operational metadata | scoped service identities; Infra runtime when built | APPROVED EARLY F1 SCOPE / implementation unverified; per-provider readiness separately gated |
| Central Audit (future repo not confirmed) | Central protected security/compliance event collector and schema; domain logs remain domain-owned | Infra, optional Identity actor context | APPROVED DIRECTION / NOT VERIFIED RUNNING |
| Billing Foundation (future repo not confirmed) | Plan/price/order/subscription/entitlements/event authority | Reference to tenant/subject IDs, payment adapter only after compliance | APPROVED DIRECTION / NO LIVE PAYMENTS |
| [gateway](https://github.com/E7A-RU/gateway) | Standalone WireGuard family/device access control plane and product-owned records on Butlerov | Existing WireGuard dataplane, Infra host/network safeguards; Core optional; Identity planned | README in main; Draft PR #1 adds documented TypeScript contracts, mock simulator/API adapter and written tests (IMPLEMENTED SOURCE; execution NOT TESTED); no live WireGuard helper/UI/DB or running Gateway. [Charter](gateway-product-direction.md) |
| Games (direction; repo not created) | Independent game product portfolio; each approved title owns gameplay, assets, domain state and releases | Optional Core, Identity, Platform and Billing by game needs | OWNER-REQUESTED DIRECTION / concrete titles and repos PROPOSED |
| Planner (future repo not confirmed) | Domain plans/tasks and optional owned DB | Core + Identity only if login required | PROPOSED; do not create prematurely |

**Status discipline:** conceptual APPROVED describes an owner-approved direction; PROPOSED describes options; IMPLEMENTED requires source reference; TESTED requires exact-SHA tests; RUNNING requires timestamped runtime health/ownership evidence; DEPRECATED requires migration owner/date. “Unknown” means evidence not collected.

## Dependency and standards pinning protocol

1. Governance owner approves a versioned baseline `governanceRevision` (full 40-hex commit SHA, optionally human tag `governance/vX.Y.Z` pointing immutably to it). No actual tag is claimed here.
2. Each product records revision in its own manifest/release evidence, plus `coreUIVersion`, `coreServerVersion` (if used), Kit version, and Identity/Platform contract versions **only when used**. `null` / absent is valid for optional dependencies.
3. Change classifier: PATCH = clarification without new obligation; MINOR = backwards-compatible obligation; MAJOR = breaking cross-product or compliance obligation. Major changes need ADR, explicit migration window and sign-off.
4. Laboratory future checker compares *pinned* docs and manifests, emits PASS/FAIL/UNKNOWN/SKIPPED with rule IDs; its proposed PRs require product maintainer review.
5. Exceptions include rule ID, affected repo, owner, rationale, risk, approval, expiry (date), compensating control and rollback. On expiry automatically flag for review, **never silently waive**.
6. Product release is independent of governance or Core release. A standards revision alone never triggers deployment.

## Suggested migration order / separate PR ownership

| Priority | Repo | Proposed separate PR | Acceptance / guardrail |
| --- | --- | --- | --- |
| P0 | .github | Review governance and contradictions, freeze approved SHA | Brand Policy unchanged; decision owner; no Actions CI |
| P1 | core | Replace conflicting historical Standard v1 normative language with pinned link, preserve history | Library boundaries/Starter unchanged; compile/package checks on separate exact head |
| P1 | website | Pin revision, annotate actual Demo routes and B2C-first positioning | Website owner verifies live claims, no automatic publish |
| P1 | reader | Pin revision and verify browser-only/local-first acceptance | No registration/backend gate added |
| P1 | bot | Pin revision and record product/platform contract boundaries | Existing Bot workers/DB remain independently owned |
| P2 | infra | Add declarative sandbox/runner safeguarding proposal | Lobachevsky and KS54 isolation demonstrated before any execution |
| P1 | gateway | Review dedicated draft PR, confirm subnet/routing/secret boundary and rollback | Existing MikroTik/VDS/S-UI unaffected, no production apply without owner approval |
| P2 | laboratory | Add governance compliance **proposal** and evidence schema without production permission | QA ≠ deploy; no Docker socket/secret exposure |
| P0–P1 | identity / platform / audit / billing | Separate architecture and F1 implementation PRs after owner-approved foundation gates | No automatic production service, payment acceptance or data migration |
| Future | planner | Product-discovery ADR | No premature runtime |

Cross-cutting Brand Policy: no third-party official E7A branding without written permission; factual reference exception stays intact. External integrations must never infer consent from SSO entitlements.

## Evidence registry (must be populated on adoption)

| Record | Mandatory fields |
| --- | --- |
| Governance revision | full approved SHA, approver, approval date, policy compatibility, changelog |
| Consumer revision | repo, immutable head SHA, pinned governance SHA, versions and exception IDs |
| QA | runner identity, suite SHA, start/end, log reference, PASS/FAIL/SKIPPED/UNKNOWN, artifact digest |
| Deployment | operator, distinct authorization, product artifact digest, target environment, rollback and health timestamps |

Existing [contract examples](../contracts/standards-consumer.example.json) and [release evidence](../contracts/release-evidence.example.json) illustrate non-runtime shapes; neither is proof of a live registry.
