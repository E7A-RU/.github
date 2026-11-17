# GOV-AUTHZ-001 — Repository and Product Impact Matrix (2026-11-10)

**Status:** OWNER-DIRECTED IMPACT PROPOSAL / NOT A PUBLISHED GOVERNANCE RELEASE. Normative baseline remains 2.1.0 until new immutable release descriptor. No product is automatically adopted or authorized to deploy. Evidence comes from existing Governance portfolio ownership and dated product repository snapshots; live host/configuration state is NOT reverified by this matrix.

## Applicability contract

**Central control plane**: E7A Identity stores/approves ecosystem product rights, grants and resource relations; ZITADEL for authentication and OpenFGA as relationship engine are selected **within Identity**. Products enforce decisions against trusted resource identifiers, preserve domain-specific state and their independent DB/QA/release. Current ACL remains until proof-driven migration. No shared data DB and no cross-product global admin.

| Repo / product | Impact and required local design changes | Adoption / safety gate | Implementation state |
| --- | --- | --- | --- |
| **identity** | Canonical Person, Organizations, Workspaces; Product Trust Registry; manifest/role schema; Technical Owner/СТП; Organization Entitlement, multi-Workspace Product Installation, grants, templates, policy API; ZITADEL + OpenFGA PoC | ADR/security/privacy/Infra admission, synthetic tenant/grant isolation, issuer mapping, restore, policy version/revoke and backpressure QA; reconcile Identity PR #2 with this normative change | Documentation only; no IdP or Policy Runtime deployed |
| **core** | Optional neutral `identity-contracts`, React UX and server enforcement adapters; Starter emits manifests/mock conformance fixtures; E7A Design System | Separate versioned Core package PR, exact published pinned versions, API compatibility and product-specific adoption; do not turn Core into auth runtime | Existing Core package does not imply new SDK is released |
| **website** | Public browsing/contact/demo remains anonymous; account/admin experiences may optionally use Identity when approved | Preserve public routes, SEO, privacy, no login wall; no forced shared runtime, independent Website release | No Identity migration implied |
| **reader** | Local file import/reading/storage anonymous; optional future sync entitlement/Identity and consent only | Test offline/no-upload and standalone mode; privacy threat model for sync and separate product migration | No current auth requirement |
| **bot** | Versioned Bot resource manifest, tenant and flow relations, Installation and roles; preserve existing product-side permission checks during phased central grant migration | Shadow/dual checks, no cross-tenant grant, token/connector secret custody, session migration, DB/worker/outbox regression and rollback; keep Bot runtime ownership | Existing local ACL must not be deleted by policy PR |
| **planner** | Map stable Planner principals/resources to canonical Person and installation grants, with ownership retained | Keep stable IDs, local state, tenant separation, explicit owner-approved pilot and reversible migration | Starter/design state separate from live Identity |
| **gateway** | Control-plane device/peer permission types and registration; privileged risk R3; product/service principals separated | Online fail-closed, step-up MFA, independent break-glass, no change to existing WireGuard/Butlerov without separate owner/Infra approval | No live host/network changes |
| **platform** | Connector/credential permission declarations and target/operation grants use central authority; preserve Platform operation checks, idempotency, provider and tenant constraints | No circular IdP–Platform bootstrap, consent and secret-broker boundaries, scoped service principals, durable outbox and reconcile; separate delivery acceptance | Planned independent runtime |
| **laboratory** | Developer/product registration requests, trusted product manifests, role schema review and conformance/QA evidence; no ability to self-approve platform security | Sandbox separation, no product-root powers, peer product review gates and protected product releases | Planned factory/QA |
| **infra** | IdP/PDP DB/keys restore, network isolation, service health/observability, scoped deployment operator rights | No production secrets/host changes from documentation; independently approved resource admission and per-change deploy authorization, emergency path not dependent on failed Identity | Runtime/host proof required separately |
| **automation** | Read-only Governance/Product Trust impact and Draft PR proposals; does not assign privileges or write grants | No broad GitHub App permissions, no auto-merge, hosted Actions or deployment authority | No changes to existing observer/poller |
| **admin-template** | Optional UI components for Organization/Installations/Policies/Audit/technical ownership in E7A Design System | Template only; no privileged runtime, no substitute for backend checks | Documentation/bootstrap |
| **.github / governance** | Normative central authority, release descriptor, impact/standards propagation and audit of consumer adoption | Owner ADR review → normative source commit → immutable descriptor release → reviewed consumer PRs; preserve historic 2.1.0 | New version pending |
| **billing foundation (not yet dedicated repo)** | Billable plans/orders/subscriptions emit authenticated entitlements; Identity owns effective grant lifecycle but not payments ledger | No payment callback creating admin access, idempotent source event reconciliation, no real financial credentials without compliance | Planned only |
| **central audit (not yet dedicated repo)** | Protected grants, policy changes, product registration, organization verification and privileged access recorded with provenance | Local durable events, authenticated handoff, tamper evidence and outage/risk policy; no common PII DB | Planned only |
| **games / future products** | Anonymous games optional; online identity-enabled products adopt product registration, installs and policy conformance | Age/UGC/security review, no universal account wall | Product-specific |

## Cross-repo compatibility and transition rules

1. New `Product Trust` does **not** confer release, cluster or customer data privileges. IAM/keys/tenant boundary changes remain R2+ unless an individual exception is owner-approved with retained evidence.
2. Current Bot/Planner/Gateway rights remain enforcement sources during transition; no double-authority silent ALLOW. A central-permission migration requires explicit cutover semantics, shadow decision comparison, backfill/reconciliation, downgrade/rollback procedures and audit. Never remove old ACL before parity tests.
3. Product owners remain accountable for resource domain invariants, including whether a resource belongs to an installation/workspace and whether a write is lawful in product state. Identity relation alone cannot bypass domain state.
4. E7A Organization vs ZITADEL Organization is a conscious boundary. No one-to-one mapping for all users or B2C Workspaces. Corporate SSO is introduced only by approved requirement and lifecycle tests.
5. Personal Workspace creation does not create an Organization. One installation owns one Organization and covers multiple explicitly scoped Workspaces belonging to it; cross-organization use must have separate grants. Tenant/isolation semantics remain distinct from both.
6. New Core packages are proposed, not automatically distributed. Current Core pin and published versions remain unchanged. Adoption across consumers is exact-version, local QA and separate release.
7. R0 anonymous/offline continues without dependency on IAM; R1 short scoped lease may be added only after measured revocation/failure tests; R2/R3 fail closed. Cache TTL must never extend privilege beyond authorized lease/freshness.
8. Owner-requested policy changes to Governance need **their own versioned release**. Existing consumers of 2.1.0 are not retroactively noncompliant until targeted reviewed adoption and documented migration gates.

## Required owner/security acceptance before normative release

- [ ] Confirm architecture wording in [GOV-AUTHZ-001](adr/GOV-AUTHZ-001-centralized-authorization.md) matches prior owner instruction, with no weakening of published Governance release.
- [ ] Select release version after compatibility assessment; **do not silently label this as 2.1.0 or assume 2.2.0 vs 3.0.0**.
- [ ] Review this matrix for all affected products and the distinct product-owned runtime migration plan.
- [ ] Review `identity-and-trust.md`, `ownership-and-data.md` and `backend-reference-architecture.md` changes for consistent terminology.
- [ ] Source exact-SHA QA (links, whitespace/JSON, security doc review) then immutable release descriptor in separate reviewed publication, followed by targeted consumer PRs.
- [ ] Verify no auto-release, broad CI permissions, production identity/keys or personal data are introduced.

**No single documentation PR is a global migration.**
