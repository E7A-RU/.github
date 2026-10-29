# E7A Product Charter — заполнить для каждого нового продукта

> **TEMPLATE / NOT AN APPROVAL.** Продуктовый документ ссылается на [принятый Governance](../README.md), [жизненный цикл](../product-lifecycle-checklists.md) и актуальный [release descriptor](../standards-release.json). Это **не** редактируемая копия общих правил; исключения оформляются отдельным ADR с владельцем и сроком.

## Identity, ownership and status

- Product ID / short name: `<product-id>`
- Repository / proposed URL: `<repo-or-tbd>`
- Product Owner: `<subject-or-tbd>`; Engineering Owner: `<owner>`
- Data Owner / Privacy contact / Support owner: `<owners>`
- Current status: `IDEA | DISCOVERY | CHARTER-APPROVED | DESIGN | IMPLEMENTED | TESTED | STAGING-VERIFIED | RUNNING-VERIFIED`
- Last verified evidence: `<SHA / test log / timestamp or UNKNOWN>`
- Governance revision: `<approved-full-commit-sha>`
- Related ADR/decision IDs and exceptions: `<refs>`

## User and business scope

- Problem / user journey / target audience and first country/language:
- B2C or B2B route; expected value; proof and measurement:
- Alternatives, overlap with existing product, and reason for separate product:
- In scope / explicit non-goals:
- Candidate for M1? `YES / NO / UNDECIDED` and discovery criteria:
- Monetization: `FREE / PROPOSED / SUBSCRIPTION / ONE-TIME / B2B / UNKNOWN` (financial/legal approval separate):

## Engineering architecture

- Kit: `Browser-only | Full-stack | Service/Worker`; [corresponding detailed implementation roadmap](../kit-implementation-roadmaps.md) chosen and accepted; version of optional Core Starter:
- Core SDK package versions and lockfile policy:
- Product-owned API, domain DB/retention, migration strategy; [PostgreSQL-first default](../backend-reference-architecture.md) for stateful backend and explicit exception ADR if deviating:
- Auth model `Anonymous / Identity opt-in / Identity required for named user actions`:
- Person vs Account vs SecurityActor and tenant/workspace scope:
- External dependencies: Identity, Platform capabilities, Audit, Billing, external providers, all with version and failure mode:
- Event/API contracts, idempotency, retry semantics and permissions:
- Threat model, privacy/data residency, secrets, rate/abuse limits:
- Accessibility/mobile/browser coverage; brand assets from Core and rights review:
- Staging/production environment resource requirements; Infra/SRE operator:

## Release / operational acceptance

- MVP user outcome and measurable acceptance tests:
- Negative/security tests, reproducible build and exact SHA evidence:
- Backup/restore/RPO/RTO where stateful:
- Monitoring/incident response, recovery path and customer support:
- Risks, mitigations, expiration of exceptions:
- Production approval requested only via immutable release record; no automatic deploy.

## Decisions

| Question | Options / rationale | Owner decision / status | Date |
| --- | --- | --- | --- |
| <question> | <tradeoffs> | PROPOSED | <date> |

**Signature:** owner approval is a separate recorded decision and does not follow automatically from filling this template.
