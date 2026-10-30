# Repository Impact Matrix and dependency register

**Status: current-source adoption register plus historical migration plan; documentation merges observed, runtime acceptance separate.** Important: historical queue below is superseded in sequencing by [final owner baseline](final-architecture-baseline-2026-10-27.md): Identity, Platform, Audit and bounded Billing enter F1; operational status remains unverified. Canonical source is [governance](README.md); consumers pin the **released immutable Governance v1.1.0 revision** `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`, not a moving branch. More recent organizational documentation changes do not silently create a new version.

**Internal automation repo CREATED / RUNTIME NOT VERIFIED:** [E7A-RU/automation](https://github.com/E7A-RU/automation) — private extensible self-hosted engineering automation service. First module: `governance-sync`, proposing scoped Draft PRs after approved Governance revisions. Later modules may propose product catalogue, `.github` public profile, README/AGENTS/status docs, safe compliance/release evidence changes. [Charter](engineering-automation.md). Not a customer product or running service: main contains README only, [Draft PR #1](https://github.com/E7A-RU/automation/pull/1) proposes plan-only source. GitHub App not registered or installed.

## Components and ownership

| Component / repo | Responsibility and data ownership | Dependencies | Verified disposition / next action |
| --- | --- | --- | --- |
| [.github](https://github.com/E7A-RU/.github) | Governance, brand policy, architecture decisions; no runtime data | none | Governance v1.1.0 approved and released via [descriptor](standards-release.json); public profile updated in merged PR #7; no deployed runtime |
| [core](https://github.com/E7A-RU/core) | `@e7a-ru/ui`, `@e7a-ru/server`, types, optional Starter; **no** Identity DB | Published package dependencies for consumers, not shared runtime | Source/docs exist; each release must be verified separately |
| [website](https://github.com/E7A-RU/website) | Public marketing, catalog, demo entry and lead intake; its own records if any | optional Core UI, own hosting | Consumer governance-pin proposal; verify production separately |
| [reader](https://github.com/E7A-RU/reader) | Browser local-first reading/user files; no mandatory server DB | optional Core UI; browser runtime | Keep anonymous offline/local behavior; acceptance test |
| [bot](https://github.com/E7A-RU/bot) | Bot domain logic, tenant ACL, messaging scenarios and owned persistence | own API/DB/workers; optional Core and external providers | No assumed migration to Platform; compatibility assessment |
| [laboratory](https://github.com/E7A-RU/laboratory) | Product factory specs, runner evidence, approvals, Developer/Partner/Staff DX | Core Starter + future Identity optional by phase; Infra sandbox | Capability is conceptual until runtime evidence; create pinned standard consumer check proposal |
| [infra](https://github.com/E7A-RU/infra) | Compute/network/TLS/secrets/storage provisioning and controlled deployment; not product business data | product manifests; owner approvals | Protect KS54/Lobachevsky; no changes outside separate Infra PR |
| [identity](https://github.com/E7A-RU/identity) (repository exists, empty at audit) | Person/org identities, assurance, SSO, access policies and own DB | Mature IdP + own Person/Claims/Policy, Infra runtime when built | APPROVED EARLY F1 SCOPE / implementation unverified; not mandatory to Reader |
| [platform](https://github.com/E7A-RU/platform) (repository exists, empty at audit) | Delivery, trusted Jobs, Connectors, Results and own operational metadata | scoped service identities; Infra runtime when built | APPROVED EARLY F1 SCOPE / implementation unverified; per-provider readiness separately gated |
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
| P2 | [planner](https://github.com/E7A-RU/planner) | Design PR #1 plus Core Starter in Draft; independent product approval | No unapproved MVP / production runtime |

Cross-cutting Brand Policy: no third-party official E7A branding without written permission; factual reference exception stays intact. External integrations must never infer consent from SSO entitlements.

## Evidence registry (must be populated on adoption)

| Record | Mandatory fields |
| --- | --- |
| Governance revision | full approved SHA, approver, approval date, policy compatibility, changelog |
| Consumer revision | repo, immutable head SHA, pinned governance SHA, versions and exception IDs |
| QA | runner identity, suite SHA, start/end, log reference, PASS/FAIL/SKIPPED/UNKNOWN, artifact digest |
| Deployment | operator, distinct authorization, product artifact digest, target environment, rollback and health timestamps |

Existing [contract examples](../contracts/standards-consumer.example.json) and [release evidence](../contracts/release-evidence.example.json) illustrate non-runtime shapes; neither is proof of a live registry.

## Governance consumer adoption — verified GitHub PR outcomes (2026-10-30)

**Release target:** `Governance 1.1.0` at full immutable normative SHA `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645` ([release descriptor](standards-release.json)). The original v1.0.0 baseline remains history. **DOCUMENTATION SOURCE MERGED is not TESTED, RUNNING or production-compliant.** Each merged consumer contains `governance-consumer.json`, exact revision pin, product-specific `governance-adoption.md` and an updated README; future E7A Automation can use these as a **read-only pointer**, not proof of effective controls.

| Consumer | Status / exact merged GitHub PR | Scope and next gate |
| --- | --- | --- |
| [core](https://github.com/E7A-RU/core) | **MERGED** [PR #25](https://github.com/E7A-RU/core/pull/25), squash `7b754eec7f1a48a4105cfe4db098d642758a3c36` | Shared SDK/Starter and package docs, local docs-validator source; no automatic package publishing or consumer deploy |
| [reader](https://github.com/E7A-RU/reader) | **MERGED** [PR #7](https://github.com/E7A-RU/reader/pull/7), squash `cb25fd14f7d2c48482304915664083efff003975` | Browser-only privacy preserved, Dependabot PR proposals only; no automatic UI adoption |
| [bot](https://github.com/E7A-RU/bot) | **MERGED** [PR #6](https://github.com/E7A-RU/bot/pull/6), squash `b936be0173fd37275d7577de278c3a6dc3b84628` | Bot owns current API/DB/worker; Platform connector extraction requires parity, QA and separate PR |
| [infra](https://github.com/E7A-RU/infra) | **MERGED** [PR #1](https://github.com/E7A-RU/infra/pull/1), squash `a102e5bdecaa65debe7250721e8d93afb9c4564b` | Architecture/recovery charter; no infrastructure provisioning or host mutations |
| [laboratory](https://github.com/E7A-RU/laboratory) | **MERGED** [PR #1](https://github.com/E7A-RU/laboratory/pull/1), squash `973db5f9dbc600004a3af34a13f6ec85b82b5832` | Product Factory documentation and archived concept; no Wizard/QA runtime activated |
| [website](https://github.com/E7A-RU/website) | **DRAFT — Git-mergeable but operations gate pending** [PR #57](https://github.com/E7A-RU/website/pull/57) | Documentation reconciled with latest `main`, 0 commits behind, no deploy script change; **verify on Lobachevsky that historical reset-first cron remains paused** before merge |
| [planner](https://github.com/E7A-RU/planner) | **DRAFT** [PR #1](https://github.com/E7A-RU/planner/pull/1) | Design + executable Starter together; product MVP scope still awaits owner approval and exact-head QA |
| [gateway](https://github.com/E7A-RU/gateway) | **DRAFT** [PR #1](https://github.com/E7A-RU/gateway/pull/1) | PostgreSQL/TypeScript source, no verified helper/runtime/security/DB integration; preserve Butlerov/WireGuard/S-UI |
| [automation](https://github.com/E7A-RU/automation) | **DRAFT** [PR #1](https://github.com/E7A-RU/automation/pull/1) | PLAN_ONLY source; TypeScript devDependency/exact-head tests and GitHub App permissions not yet accepted; no write tokens |
| [identity](https://github.com/E7A-RU/identity) | **NOT ADOPTED (empty at last inventory)** | Initial owner-approved bootstrap needed before normal PR flow |
| [platform](https://github.com/E7A-RU/platform) | **NOT ADOPTED (empty at last inventory)** | Initial owner-approved bootstrap needed before normal PR flow |

### Separate PR queues — no implied auto-merge

- [Core Starter #27](https://github.com/E7A-RU/core/pull/27): optional product release CLI **source-only**, no executable exact-head QA; Core package publication and Infra production remain separate.
- [Reader Dependabot #8](https://github.com/E7A-RU/reader/pull/8): Next.js/eslint-config-next update; compatibility checks, browser/privacy acceptance and lockfile QA pending.
- [Website Dependabot #60](https://github.com/E7A-RU/website/pull/60) and [#61](https://github.com/E7A-RU/website/pull/61): Next.js and React runtime patches respectively; independent local/build/visual/rollback review and **Website cron safety readback** pending. Do not infer production promotion from mergeability.
- Games is a Governance product direction, **not** a registered game repo, release or running application.
- No hosted GitHub Actions CI was introduced during adoption. Core's existing manually owner-authorized **package publishing only** workflow remains the sole documented exception.

**Static documentation check limitations:** the merged documentation PRs were reviewed against the exact GitHub source head with local links validated (Core 70; Reader 10; Bot 51; Infra 6; Laboratory 9 after fixing three archived links). **Executable local/Lobachevsky tests were not run** as part of this documentation sweep; do not record them as PASS. Product runtime, actual deployed SHA, recovery and F1/F2 readiness need independent evidence. Future updates to this table should be generated as Draft PR proposals by E7A Automation after approval, not direct automatic commits to the organizational main.

See [E7A Roadmap](roadmap.md), [product release checklist](product-lifecycle-checklists.md) and [standards propagation](standards-propagation.md).
