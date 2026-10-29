# E7A Product Lifecycle — обязательные checklist/gates

**Status: APPROVED GOVERNANCE PRINCIPLES / CHECKLIST TEMPLATE PROPOSED FOR ADOPTION.** Source: [Governance](README.md), [roadmap](roadmap.md), [Product Factory](product-factory-and-kits.md), [Infra](infra-operations.md). Checkboxes below represent **requirements to verify for each product**, not claims about already completed tasks. Evidence = exact SHA/artifact digest/run URL or dated operator readback. A green box without evidence is not PASS.

Для нового продукта начинайте с [шаблона Product Charter](templates/product-charter.md); для каждого релиза оформляйте [Release Gate и evidence](templates/release-gate.md). Заполнение шаблонов не даёт права deploy.

**Kit-specific обязательный маршрут:** [Browser-only → Full-stack → Service/Worker: полный roadmap и gates для каждого](kit-implementation-roadmaps.md). Для stateful Full-stack/Service — [PostgreSQL-first Backend Reference](backend-reference-architecture.md). Для PR-автоматизации стандартов — [GitHub App costs/resources](github-app-cost-and-resources.md). Чеклисты этого документа выполняются **в дополнение** к Kit-specific, а не вместо них.

## Ownership and applicability

Each independently deployable product has an explicit **Product Owner, Engineering Owner, Data Owner, Security/Privacy contact, QA reviewer, Infra operator and Production approver**. People may temporarily overlap, but roles must be visible; owner-only critical approvals are an explicit temporary risk exception, not two independent humans. Products use one of three **Kit** contracts: Browser-only, Full-stack or Service/Worker. Choose no mandatory shared runtime unless functional need and failure semantics are documented.

### Lifecycle states

`IDEA → DISCOVERY → CHARTER-APPROVED → DESIGN → IMPLEMENTED → TESTED → STAGING-VERIFIED → APPROVED FOR PRODUCTION → RUNNING-VERIFIED → MAINTAINED → DEPRECATED/ARCHIVED`.

A repo, a Draft PR, a green static scan, a package tag and a production health sample are different evidence categories. Phase advancement is **per product and per release**.

## Gate A — discovery and product authorization

- [ ] Problem, user segment, jurisdiction/language, expected benefit and alternatives written; B2C-first priority and B2B exception described.
- [ ] Product idea compared to existing products; no duplicate Shared Identity/Platform/Billing/Laboratory/Infra responsibilities.
- [ ] Target operation is legitimate and supports privacy-by-design; record basic threat/abuse case.
- [ ] Product Owner decides independent product vs Website section vs feature of existing product.
- [ ] Success metrics, feedback process and minimal test scenario defined before technical build.
- [ ] Names, domain/slug, public promises, legal/IP, brand rights and pricing assumptions reviewed; Games and Gateway do not imply public runtime.
- [ ] Scope/non-goals and M1 candidacy explicit. Product idea approval is not deployment authorization.

**Evidence:** Product Charter + initial roadmap + owner decision ID + known regulatory blockers.

## Gate B — architecture, Kit and data boundary

- [ ] Select Browser-only / Full-stack / Service/Worker with reason; [Kit-specific roadmap](kit-implementation-roadmaps.md) selected and linked from Product Charter; Core Starter imported/pinned without forking shared template.
- [ ] Repository and ownership map; `governanceRevision` pin to immutable accepted SHA or tag; product local ADR does not weaken organization policy.
- [ ] Product identities distinguished: Person, Account/OIDC `sub`, SecurityActor, Organization, Workspace, tenant, product resource. No implicit automatic merges.
- [ ] Domain DB/storage and schema/migrations owned by product; **PostgreSQL-first** for stateful Full-stack/Service with separate roles and checksummed SQL migrations (unless a justified ADR); Browser-only has **no mandatory DB**. Infra provisions resources without taking ownership of domain data.
- [ ] Shared capabilities (Identity, trusted Jobs, Delivery, Connectors, Audit, Billing) explicitly **optional or required by user use case**, with versions, permissions and outage behavior.
- [ ] API/event/SDK contract versions, compatibility/deprecation policy and consumer migration mapped.
- [ ] Operations permissions scoped to Product + Organization/Workspace + Actor + Capability; privileged paths MFA/audit/expiry.
- [ ] External connectors require capability matrix, credentials owner/secret broker, consent, rate limits and provider API validation.
- [ ] Personal data types, region/residency, lawful basis/consent, minimization, storage TTL, retention/export/erase, backup lifecycle, cookie/session policy documented.
- [ ] Supply-chain/third-party licensing of code, media, fonts and brand assets checked; no unapproved logos or artifacts.
- [ ] Dependency graph excludes boot/recovery cycles; fallback/degraded modes designed for outages.

**Evidence:** architecture ADR, threat model, Product Manifest, contracts, data map, exceptions with owner/expiry.

## Gate C — engineering and local QA

- [ ] Reproducible dependency installation with frozen lockfile, supported toolchain and pinned versions.
- [ ] Typecheck, lint, unit/integration tests, schema checks, build, dependency audit and security negative cases applicable to chosen Kit.
- [ ] Browser-only: no silent upload, anonymous availability, offline/degraded mode, local file format/security limits, accessibility and mobile browsers as relevant.
- [ ] Full-stack: migrations on disposable DB, tenant isolation, authN/authZ, idempotency, backup/restore rehearsal, operation observability.
- [ ] Service/Worker: job dedup/lease/cancel/retry, DLQ/UNKNOWN semantics, poison-message/broker disconnect/egress limits.
- [ ] Interactive Demo: simulation plainly labeled; no real third-party messaging without authorized provider/account.
- [ ] Untrusted code runs in restricted sandbox, never privileged Platform/Infra worker or direct Docker socket.
- [ ] QA on **exact commit SHA** using approved local or isolated self-hosted runner; `SKIPPED`, `UNKNOWN` and missing logs **never PASS**.
- [ ] Results, timestamps, log ref, test suite version and artifact digest recorded; all severity-critical findings closed or exception owner-approved.

**Evidence:** immutable check report + security review + artifact provenance. Hosted GitHub Actions CI remains prohibited; Core's separately authorized manual publishing workflow is not CI.

## Gate D — staging and operational readiness

- [ ] Environment `Development → Staging → Production` distinct; isolated QA VM/sandbox is not staging.
- [ ] Domain/DNS/TLS/ingress plan, ports, network/egress, quota, CPU/RAM/disk limits, secret permissions, data stores and third-party dependencies enumerated in Infra manifest.
- [ ] Staging uses reviewed immutable build with release SHA/digest, smoke/health, E2E and negative tests; no production secrets in staging.
- [ ] Stateful products: **off-host backup**, restore drill, RPO/RTO per component, migrations and schema rollback approach.
- [ ] Current host state and existing service blast radius inspected; prevent impacts on KS54/Lobachevsky, Butlerov/WireGuard, other products.
- [ ] Security/degraded-mode simulation: Identity/Audit failure, unavailable provider/DB/broker, revocation, resource exhaustion, rollback.
- [ ] Incident operator, alerting, support/feedback owner, data retention and decommission path assigned.

**Evidence:** staging verification, recovery/rollback logs, Infra plan/diff, owner review of risk.

## Gate E — production approval and launch

- [ ] Release record includes **product, repo, full SHA, artifact digest, target environment, exact changes, user/tenant impact, risk tier, QA and rollback evidence**.
- [ ] Approval is **separate explicit owner decision**, bound to immutable release record, target and expiry; GitHub/ Laboratory display the **same** record, not two independent authorizations.
- [ ] R0/R1 auto-merge rules (when operationally authorized) **do not grant deployment**; R2+ contract/critical changes require appropriate human review.
- [ ] Critical owner-only review exception is recorded with rationale/expiry/compensating checks; external independent reviewer when risk requires one.
- [ ] Infra independently verifies approval, artifact/signature, target, credentials and privileges; no automatic PR-merge deploy and no production Docker socket from Laboratory.
- [ ] Deploy plan, health/smoke threshold, user communication, rollback/forward fix and emergency access ready.
- [ ] **No public RUNNING claim until timestamped runtime proof.** For website/catalogue, accurately distinguish idea, demo, test, limited pilot and live product.

**Evidence:** production approval ID and operator deployment/health evidence; no claim until executed.

## Gate F — maintenance, updates and retirement

- [ ] Health/incident coverage and degradation goals measured; vulnerability/patch management with per-consumer versioning.
- [ ] Governance version updates trigger a **Draft PR proposal**, impact report and human review; no automatic product deployment.
- [ ] User feedback, accessibility, privacy incidents, billing disputes and operational cost reviewed.
- [ ] Each release requires repeatable testing and safe rollback; no silent whole-ecosystem package upgrade.
- [ ] On retirement: export/delete domain data by retention requirements, revoke credentials/entitlements, remove endpoints and DNS through owner-authorized Infra plan; immutable audit retained only per legal/security policy.

## Additional safety gates by product class

| Product | Additional non-negotiable checks |
| --- | --- |
| Games | Age suitability, minors/parental privacy, game-content/music/art rights, app-store policy, fraud/UGC/moderation when relevant; no paid random rewards by default |
| Gateway | Real network plan and isolation, key custody, out-of-band management, live route/NAT/IPv6/DNS leak checks; owner approves each change to existing WireGuard/Butlerov |
| Bot Studio | Existing API/DB/worker isolation, dialog parity, migration of connector tokens/permissions and rollback before general Platform extraction |
| Website | B2C/B2B conversion path, RU/EN language, WCAG/accessibility, search indexing, honest public promises, controlled demos |
| Reader | Browser-only/local-first, no compulsory account, no upload of user documents, format/parser DoS budgets |
| Planner | Owner-approved MVP scope, planning data privacy, calendar import/export permissions, conflict resolution |
| Identity/Audit/Platform/Billing | Key rotation, privileged action audit, fail-closed paths, tenant-bound credentials, financial/identity reconciliation and tested emergency restore |

## Required product release evidence summary

```yaml
# illustrative checklist record; NOT a runtime API
productId: example
governanceRevision: b39ed4aabf97e45edaa0762a29ab57c69f8bbafd
kit: browser-only
sourceSha: "<full-reviewed-source-sha>"
artifactDigest: "<immutable-digest-or-n/a-for-doc-only>"
riskClass: R1
qa: {status: UNKNOWN, logReference: null}
staging: {status: NOT_TESTED}
production: {approval: NOT_GRANTED, status: NOT_DEPLOYED}
exceptions: []
```

**Validation semantics:** above `qa.status: UNKNOWN` is deliberately **not** a pass. Every actual release requires unique evidence. The template must not be copied as a green status or used to infer owner approval.

Related: [Roadmap](roadmap.md), [standards propagation](standards-propagation.md), [Product Factory](product-factory-and-kits.md), [Infra](infra-operations.md).
