# E7A Final Architectural Baseline — owner decisions, 2026-10-27

**State: OWNER APPROVED ARCHITECTURAL DIRECTION; GOVERNANCE DRAFT, not yet released in main.** Consolidates [rounds 1–10](owner-decisions-2026-10-27.md) and subsequent eight clarification decisions. No code or runtime is certified by this document. Historic decisions remain accessible; where plans conflict, this owner-approved direction takes precedence for new proposals, while a future accepted standards release must update all normative cross-references.

## Constitutional invariants

1. E7A is B2C-first product-and-engineering ecosystem with independent product ownership, domain data and releases, B2B delivery as a secondary business direction. Architecture/F1 first without indefinite platform-building; Website sales/discovery improves in parallel. Reader is a **candidate**, not approved M1 flagship.
2. Core is versioned UI, SDK, typed contracts, server libraries, design system, optional Starter; **not** operating backend/Identity/Infrastructure. Brand use requires prior written authorization under [Brand Policy](../BRAND_POLICY.md); public availability does not grant open-source rights.
3. F1 Foundation-integrated, F2 recoverable production-ready by individual capability, M1 Product-in-Use with real use/feedback and safe repeat release. Identity, Platform's Delivery/Jobs/Connectors/Results, central Audit and bounded Billing are in **initial foundation scope**, not deferred until after M1. Implementation rollout is phased.
4. Laboratory is separate factory/QA/release-evidence and invite-only developer portal. QA untrusted work runs in isolated VM on Lobachevsky; environments Development → Staging → Production; Infra uses node-aware interfaces, implements deployments separately, and must protect KS54. QA VM is **not** the same as staging. Multiple nodes are supported by design, not necessarily provisioned.
5. GitHub-hosted CI Actions prohibited (separately owner-authorized package publication exception only); self-hosted/local exact-SHA checks. Risk-classified R0/R1 automatic merge allowed only after an separately approved and tested policy, never implied operational today. R2/R3 sensitive changes require human review. **All production deploys require distinct explicit owner approval**, recorded with immutable SHA/digest, target and time-bound authority in one release record surfaced by Laboratory/GitHub and independently enforced by Infra.
6. Founder is presently the only human performing E7A governance/engineering roles; **4 FTE is aspirational capacity, not personnel.** For now **owner-only approval for critical releases is explicitly accepted as a temporary exception** to the former two-human approval recommendation, not a claim of independence. Make exception visible, time-bounded and reviewed each critical deployment; require stronger automatic evidence (risk/threat model, immutable build, tests, backup/rollback), out-of-band recovery, and no agent self-approval. No unreviewed self-hosted escalation. Revisit when a second reviewer/operator is available or risk increases. For high-impact migrations/keys, defer release if controls insufficient.
7. No automatic production apply, no unauthorized consumer repo modifications and no new production service merely by ratifying Governance.

## Tenant and namespace ontology — approved semantics

- `Person` is a canonical *physical human*, stable identity independent from account or session. `ServicePrincipal` is a separate nonhuman subject. `AuthenticationAccount`/OIDC `sub` and `SecurityActor` remain distinct concepts; **do not assume OIDC sub = Person ID**.
- A Person may have a **Personal Space** and memberships in multiple **Organizations**. Each Personal Space or Organization is an ownership root. It may hold multiple **Workspaces** as collaboration/billing/security contexts. Products bind resources to explicitly scoped workspace/ownership roots, but preserve their own domain storage and resource ACL.
- `Tenant` is a **security/isolation boundary**, not automatically equal to a company, person, workspace or PostgreSQL database. Initial mapping and immutable tenant IDs, sharing/delegation/cross-space access, organization transfer/closure and workspace lifecycle require an implementation ADR before API schema freeze. Do not merge tenants via Person merge.
- Authorization request inputs must distinguish actor, acting-for/delegation, owner scope, resource, capability and entitlement. Least privilege must hold across Platform, Billing, Identity and products.

## Identity and security — approved direction

- **Mature OIDC/OAuth2 IdP** handles authentication protocols, sessions and federation; **E7A-owned Person/Claims/Policy model** owns canonical person resolution, assured attributes, memberships, scopes/roles and controlled disclosure. Choose product/provider after proof of recovery, key rotation, multi-region/legal implications and migration feasibility; avoid self-written login cryptography.
- Passkeys-first target UX; initial email/password + WebAuthn/passkeys + MFA + OIDC, then VK, Yandex and Google through approved upstream integrations. Linking external accounts requires proof of control; no automatic merge on email. Preserve distinct MATCH, CREATE, UNRESOLVED, CONFLICT, reversible merge/split handling and provenance. Service identities are not Persons.
- Expanded profile is composable; Identity is not universal owner of operational domain data. Attributes can be identity-owned or linked with explicit provenance, trust, lifecycle, consent/legal basis, minimization and access policy. Personal data localization, cross-border processing and deletion/backup consistency are compliance gates.

## Audit foundation and resilience — approved

- Central secure Audit collector from early foundation, shared versioned schema and permitted central record store with ingestion authentication, tamper evidence, retention, access segregation and redaction. Domains still own original business records/operational logs. Audit is **not** a general-purpose shared PII database.
- **Risk-based failure policy:** ordinary low-risk flows may continue with an integrity-protected durable local buffer and later reconciliation; privileged/key/security/production-critical operations fail closed unless explicit emergency break-glass path leaves independent verifiable evidence. Buffer quotas, ordering, anti-replay and recovery thresholds require operational tests.
- Secure bootstrap must avoid circular dependencies: Identity may issue primary tokens without calling Platform/Billing; Audit initial ingestion cannot depend on SGDS; Infra emergency recovery cannot depend solely on unavailable Identity/Laboratory.

## Platform + connector boundary — approved

- Delivery uses local transactional outbox/inbox and durable state transitions, idempotency, leases/retries/DLQ, provider receipts and UNKNOWN reconciliation; no blanket external exactly-once. Jobs allows **trusted** work with quotas/priorities/schedule/cancel, not arbitrary multi-tenant code.
- **Untrusted user code executes only in separately isolated Sandbox, not in privileged Platform Workers**. No unrestricted host shell, Docker socket, production secrets or intra-tenant egress.
- SDK is one small versioned connector business-facing interface with capability schema, validation, errors, result contract, plus per-provider adapter and per-customer/person/org/product connection. Distinguish SDK, platform API, adapter, provider and credential versions. Official initial providers scoped for investigation/implementation: Telegram, MAX, VK, email, Instagram; each requires verified API capabilities/permission/consent, not unsupported promises. Third-party plugins separately reviewed, signed, sandboxed and revocable.
- Workflow is versioned technical execution graph, individual operation results + aggregate batch, signed result subscriptions/events/API; product owns the **business** decision and dialog rules. Bot Studio will evolve into dialog/business-logic builder; generic connector services gradually migrate to Platform only after parity, staged migration, security and rollback evidence. No live move authorized.

## Billing foundation — approved

- Initial bounded capability: customer/tenant reference, price plans and versions, orders, subscriptions, entitlements, event contract and audit. **No real charging or payment credentials until compliance, legal/business, processing and reconciliation review.**
- Billing records commercial state; Identity relates the authorized subject/product entry, but does **not** become a ledger; products retain per-resource authorization. Do not allow payment callbacks to create unaudited admin grants.
- Eventual finance-ready design should address idempotent webhooks, refunds, disputes, tax, invoice/receipts, price change and entitlement revocation. No operating payment platform claimed.

## Availability and geographic posture

- **F2 = verified restoration, not mandatory HA cluster.** Required: isolated backups kept off Lobachevsky, test restore (including keys/credentials), declared and measured RPO/RTO per critical component, rollback/incident procedures, degradation mode and owner/operator contacts. A multi-node API does not itself constitute failover or HA.
- **Architecture is multi-region-capable by design**, but the **first commercial market and processing jurisdictions remain OPEN**, not assumed. Region, residency, tenants, secrets/keys, audits, billing/currency and provider legality must be pinned prior to storing real customer data or accepting payments. Avoid promising legal compliance in all countries merely from region fields.

## Commercial and intellectual property boundaries

- Founder-owned individual-rights structure is owner intent pending chain-of-title verification, employment/IP checks and formal legal documents. B2C subscriptions/one-time purchases + B2B contracts; no automatic open-source licenses. Contributor agreement for voluntary code contributions plus project/work agreements for commissioned work; exact enforceable terms need legal review. Brand Policy preserved.
- Planned initial trademark path is ONE combined E7AMark+E7A filing class 42; no filing, fees, registration or guaranteed availability claimed.

## Dependency graph — bootstrapping

```text
Infra physical resources / isolated network / independent secret bootstrap
  ├─ Identity IdP + Person/Claims/Policy
  ├─ Central Audit (accepts directly, resilient local buffering)
  ├─ Platform (may use Identity subject assertions; does not require Billing)
  │   └─ Delivery / Jobs / Connectors / Results
  ├─ Billing Foundation (references Identity subjects; independent commercial state)
  └─ Laboratory (uses Identity, QA VM, Audit, Infra deploy API)
Products → Core SDKs (optional versions); Product → Identity/Platform/Billing per required capability
Website public demo and Reader anonymous/local-only flows remain usable without forced login.
```

A dependency may be optional/async, but runtime boot, revoke, recovery, and failure isolation must be tested. Never assume the displayed hierarchy mandates a single infrastructure DB or universal request proxy.

## Remaining design ADR gates (not new owner-strategy questions)

- TEN-001 tenant/workspace/owner ID and cross-tenant delegation/transfer.
- IAM-001 IdP choice, OIDC subject/account mappings, MFA and account recovery, privileged bootstrap.
- AUD-001 event schema v1, fail-closed action classes, retention/tamper/replay evidence.
- PLT-001 operations state machine/outbox/inbox/idempotency/dead letters and workflows; connector capability compatibility test protocol.
- INF-001 VM/resource capacity and KS54 safeguards, secrets/key recovery, backup RPO/RTO, node control protocol.
- BIL-001 financial event semantics and legal payment activation gate.
- GOV-001 executable risk classifier, auto-merge safeguards, temporary owner-only critical approval expiry/review.
- MKT-001 first market, hosting geography, compliance, commercial entity/currency; M1 product selection thresholds.

**Evidence required before any “TESTED/RUNNING” label:** exact source SHA, approved contract version, negative tests, isolation demonstration, deployment/health evidence and recoverability record. The baseline is a decision, not a test certificate.
