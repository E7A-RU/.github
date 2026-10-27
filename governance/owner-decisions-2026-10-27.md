# Owner decisions — consolidated architecture baseline (rounds 1–10)

**Decision date:** 2026-10-27. **Status:** OWNER-APPROVED DIRECTION / DRAFT GOVERNANCE PR; not approved as a released governance standard until PR review and merge. **IMPLEMENTED, TESTED and RUNNING are not implied.**

This document supersedes conflicting *proposals* in earlier governance drafts by decision priority; it does **not** delete historical text or retroactively claim deployed services. Consult [decision log](decision-log.md), [consistency report](architecture-consistency-report.md), and [impact matrix](repository-impact-matrix.md). For any already APPROVED decision in a different document, owner reconciliation remains required.

## Authority / release principles

- E7A is an integrated product-and-engineering ecosystem, B2C-first with a real B2B offering. Product choice for M1 remains a discovery decision; Reader is a candidate, not selected.
- Establish architecture before broad product rollout. **F1** = contract/identity/platform/infrastructure integration proven by end-to-end tests. **F2** = production readiness with security, backups, restore/rollback, monitoring and operational roles. **M1** = observed real product use, feedback, reproducible repeat release. F1/F2 must be scoped per capability; one working happy-path does not certify all providers or availability.
- Core owns versioned UI, design tokens, typed SDK/contracts, reusable server libraries and optional Starter. No running central backend, product DB, Identity Provider, Laboratory or deployment authority.
- Agents may implement/review Draft PRs autonomously. Risk-classified R0/R1 changes may *eventually* auto-merge only behind approved self-hosted exact-SHA verification, branch protections and path/risk rules. R2 architectural/SDK changes need review; R3 identity/security/infra/migrations need human approval. No auto-merge system is claimed deployed. GitHub-hosted Actions **CI prohibited**; separately authorized package-publishing Actions remain the only documented exception.
- Production (including release of Identity, Platform and Billing) **always requires a separate explicit owner approval** tied to repo SHA, immutable artifact digest, environment, validity and rollback; GitHub and Laboratory refer to **one canonical release approval record**, verified by Infra. Critical releases require independent technical evidence/review. As only one human currently performs roles, two AI agents cannot substitute for two independent human approvals; seek an independent qualified reviewer when required.

## Identity / Person / Security (APPROVED target)

- Canonical stable Person is independent from login Account, AuthenticationIdentity, Session, SecurityActor, role, employee/developer/customer context and domain records. A single person may hold simultaneous or sequential context-rich relationships across many organisations and a personal space.
- Service principals are distinct from human Persons. Organisation membership, entitlements and scoped policies use RBAC + scoped permissions/policy checks; product domain ACL remains product-owned.
- Multi-login design: email/password, Passkeys/WebAuthn, MFA, OIDC federation and planned VK, Yandex, Google sign-in; **Passkeys-first UX aspiration** does not remove initial password support. Do not auto-merge accounts on matching email; prove ownership, preserve provenance and reversible conflict resolution.
- Claims/attributes store provenance, assurance, status, evidence, expiry and revocation. Expanded Person profile is a **composable view of identity-owned and linked domain-owned attributes**, NOT a central dumping ground for every product's operational data. Data minimisation governs collection, storage, APIs and disclosure.
- Avoid custom authentication cryptography. Evaluate proven IdP, verify OIDC client security and upstream provider capabilities before claiming delivery. Person match/creation/UNRESOLVED/CONFLICT semantics inspired by KS54's Person vs Security Actor ADR, **without importing KS54 records or assuming its lifecycle/source identifiers apply to E7A**.
- Invite-only Laboratory for staff/approved partners/developers. Restricted, recorded, expiring break-glass with out-of-band recovery and retrospective review. Do not pretend one-person operations satisfy two-person controls.

## Audit foundation (APPROVED scope, implementation unverified)

- Central E7A Audit service / secure ingestion and a **single versioned event envelope from the beginning**. Event ID, schema version, producer, event time, actor (authenticated/effective where relevant), person/service principal reference if permitted, product/org/tenant context, action, outcome, request/correlation/trace IDs, authorization decision, and evidence reference.
- Product domains independently own their source logs and domain records. Central Audit maintains permitted security/compliance events with access controls, tamper evidence, retention/legal deletion policy, redaction, key management, transport authentication and replay protection.
- Durable edge buffering if audit ingestion temporarily fails; for sensitive actions design explicit fail-closed or bounded fail-safe decision based on threat model. Central aggregation is not a shared product business DB.

## Platform / СГДС (APPROVED target; not RUNNING)

- **All four logical contours in starting scope:** Delivery, Jobs/Workers, Connectors and Results. A modular implementation is acceptable; four contours do not demand four servers.
- Transactional boundaries inside owned persistence (operation state + outbox/inbox where appropriate), idempotent admission, durable jobs, leases, backoff, bounded retries, DLQ, provider receipts, reconciliation and explicit Accepted/Dispatched/Delivered/Processed/Failed/Unknown. No blanket exactly-once guarantees across external APIs.
- Jobs: general task queues, schedules, quotas, priorities, cancellation and idempotency. Results: API, event streams, scoped subscriptions/signed webhooks; independent results per operation and aggregate batch result, not fake global rollback.
- All operations checked by Product + Organisation + Actor + Capability and credential owner/scope. Credentials never sent to arbitrary third-party plugin code.
- **Connector architecture:** core versioned Connector Contract/SDK for capabilities, validation, operation, error and result types; separate provider adapters (Telegram, MAX, VK, email, Instagram), individual connection instances belonging to person/org/product, separate credential broker/secret storage; distinct versions for SDK, Platform API, adapter, provider. Extensions and provider-specific behavior permitted. Official integrations require proof of supported APIs and authorized scopes. External plugins only after signing/approval, isolated execution, egress/resource budgets and revocation.
- Versioned workflow contract for sequential/parallel operations, dependencies, timeouts, compensations and partial outcomes. Workflow execution belongs to Platform; **specific business/dialogue decisions belong to products**.

## Bot Studio migration (new owner decision, target not implementation)

**Future target:** move generic connector adapters, integration execution, provider connections and neutral delivery facilities from Bot runtime into Platform Connectors **where an explicit contract/inventory shows a safe replacement**. **Bot Studio remains an independent product and evolves toward a dialogue and business-logic constructor**: visual flows, triggers, rules, state, tenant permissions, user-facing configuration and product-domain data. Bot business logic is NOT absorbed by Platform. Existing Bot operations cannot be removed before feature parity, dual-run/adapter compatibility, secret/tenant migration plan, negative tests, staged cutover, rollback and owner-approved individual repo PR. No claim of migration performed.

## Laboratory / Infra / release

- Laboratory is a distinct Product Factory, Wizard based on Core Starter, three Kits (Browser-only, Full-stack, Service/Worker), QA, temporary sandbox, release evidence, Developer/Partner/Staff spaces and Governance checker. Initial access invite-only.
- Development → Staging → Production are product environment stages; an isolated **QA VM on Lobachevsky** is the compute boundary for untrusted tests, not a staging substitute. Stage and production need separate credentials, volumes, networks and access rights.
- Infra can begin on Lobachevsky with basic control plane and strict safeguards for KS54 workloads; design multi-node API from inception but provision more nodes only when needed. Infra remains only component authorized to apply production. No direct production Docker socket in Laboratory/QA.
- Website evolves in parallel into strong conversion-oriented B2C product discovery and B2B custom automation intake; honest Demo labels; no blocking on full Platform launch.

## Billing / intellectual property (owner direction)

- **Billing Foundation is in F1 scope**: define tenant/customer/entitlement, plan, price version, subscription, order, invoice/receipt reference, payment provider capability, ledger reconciliation and audit contracts; implement minimal bounded payment lifecycle only after legal, accounting, security and processor review. Do not create a fictional fully operational billing service or start accepting payments automatically. Finance ownership is separate from Identity authentication and Platform integration mechanics.
- B2C subscriptions/one-off purchases and B2B services coexist. Legal ownership model starts with **natural person founder**; actual chain of title for existing code/assets must be verified against employment, commissioned work, collaborators and provider terms.
- Proprietary/commercial default; no implicit open-source grant. **Licensing** means explicit permissions to use/copy/modify/distribute software, distinct from copyright ownership and public availability. Separate terms for publicly installed Core SDK and third-party consumers need legal drafting; preserve BRAND_POLICY.md and authorized factual mention. Trademark filing intent remains **one combined E7AMark + E7A in Nice class 42**, subject to clearance/fees review and separate filing permission.
- **Two-track contributor policy:** simple unified contributor terms (provenance/right-to-contribute, permitted reuse, patent considerations if applicable, no automatic brand license) for contributions to E7A repositories; commissioned client/partner/staff work additionally needs separate project-specific agreement governing deliverables, confidentiality, payments, IP assignment/license, employment and data processing. Exact legal instruments NOT approved until counsel review. No contributor agreement is asserted signed.

## Reconciliation required (not hidden)

1. Prior [implementation priorities](implementation-priorities.md) deferred Identity and Platform until after M1: **superseded in planning** by early foundation Identity, Platform (four contours), Audit and bounded Billing; M1 product choice remains open.
2. Prior [Platform charter](platform-delivery-jobs-integrations.md) described general Jobs-as-a-Service as later: **superseded target scope** to start general Jobs with staged capability rollout.
3. Earlier Bot boundary [ownership](ownership-and-data.md) treated provider adapters as long-term Bot-owned: **migration proposed/approved target** now assigns generic connectors to Platform, with existing runtime untouched.
4. Audit previously described dispersed storage only: central ingestion/storage of permitted audit events is now an explicit starting capability; source logs remain owned by domains.
5. Identity older baseline described minimal Person: canonical Person stays minimal; **profile projection** is extensible and access-minimized.
6. 4.0 FTE is a **future indicative capacity**, not actual headcount; owner currently fills roles, so external independent critical review remains a real unsolved resource.
7. Billing Foundation start does not mean working Billing/charging, and "first provider adapters" does not mean official credentials or unrestricted features already exist.
8. License/contributor exact terms remain a legal **PROPOSED implementation** despite founder-approved commercial posture.

## Immediate architecture-first deliverables

P0: Governance ADRs/ownership matrix; F1 dependency/evidence milestones; threat modeling; prototype identity and audit contracts; Platform transactional operation/connector contract; Bot migration inventory; billing domain contract; QA VM resource feasibility and KS54 blast radius. P1: proven integration slice (one authorized real provider; others separately gated), owner-approved staging deployment, protected GitHub-to-Laboratory release record, Website conversion work. P2: expansion of adapters and permissions, paid flow after legal readiness, M1 product selection/use/repeat release.

**Forbidden in this governance phase:** new production services, CI workflows, package publications, payment collection, trademark filing, third-party code distribution, unapproved repo edits and production deploy/merge.
