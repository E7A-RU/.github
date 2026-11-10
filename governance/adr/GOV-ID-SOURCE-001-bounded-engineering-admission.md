# GOV-ID-SOURCE-001 — bounded source admission for E7A Identity

**PROPOSED · 2026-11-11 · NOT NORMATIVE / NOT AN ACTIVE EXCEPTION.**

Requested by E7A Owner to separate local synthetic engineering from admission of
Identity to real users. Current approved Governance remains **3.0.0**, normative
source `80b574cf01d2976b4112e3e492692b02233b9f44`; this ADR does not waive any
existing review, protected-main or GitHub requirement.

## Problem and interpretation

[Product Stewardship](../product-stewardship.md) requires appropriate engineering
review for identity code, dependencies and migrations. [Product Lifecycle](../product-lifecycle-checklists.md)
requires security evidence and risk-based reviewer separation.
[Release Policy](../release-deployment-policy.md) excludes IAM/Identity from
ordinary low-risk standing authorization. [GOV-AUTHZ-001](GOV-AUTHZ-001-centralized-authorization.md)
preserves separate review and Identity R2+ gates.

These documents do not provide an explicit independent-security-review deferral
for the current Identity engineering PRs. The existing product acceptance also
requires independent review. An Owner development instruction and the current
PRs' synthetic runtime do not alone publish that exception. Codex QA is
engineering evidence, not independent review. This proposal removes that
ambiguity through a narrow, explicitly published mechanism.

## Version classification

**MAJOR 4.0.0 proposed for Owner acceptance.** Deferring an existing mandatory
Identity review gate changes the security acceptance contract, even with a
narrower runtime scope. It is not a PATCH or a backward-compatible new
obligation. This classification does not release 4.0.0 or reclassify 3.0.0.
Other consumers retain their existing pins and rules; no bulk adoption follows.

## Proposed rule

Only **E7A-RU/identity**, local synthetic Docker engineering, may receive a
bounded **source-only admission** after this rule has become normative and
Identity has adopted its exact revision. It is not an R0/R1 auto-deploy or
ecosystem-wide exception.

Each admission is an Owner-reviewed record containing:

- Unique admission ID, product, repository, target branch, PR numbers and exact
  permitted head SHAs; finite expiry no longer than **14 days**; revocation owner.
  No wildcard SHA/range, mutable branch or automatic renewal is valid.
- Changed-path/diff summary, current risk and known findings, author and accountable
  human. A new head invalidates approval until QA and admission are updated.
- Exact-SHA Docker build/typecheck/unit/integration/negative tests, genuine frozen
  lock and image digests, commands, environment and retained log hashes.
- Proof of local-only network and synthetic data/keys; no production endpoint,
  users, credentials, privileges or existing product migration.
- Disposition of every applicable source gate. Confirmed critical or high
  authorization bypass, tenant escape, credential exposure or unsafe data
  destruction remains a source merge blocker regardless of synthetic scope.
- Applicable protected-main/GitHub rules and human Owner approval of the concrete
  tested diff. The admission cannot bypass branch protection, approve its own
  governance release, dismiss a human review, rewrite history or activate CI.
  An unprotected main is not silently treated as compliant.
- Deferred independent-security-review scope, accountable owner and stop condition:
  it is mandatory **before Identity serves real users on a server**, including
  a staging pilot. Server sandbox resource admission is still separate.

Within that exact admission, Codex may author and execute engineering QA and
perform a specifically authorized source-only merge after all remaining gates
pass. Codex must label this self-verification honestly. Existing independent
review findings cannot be ignored or reclassified by the implementation agent.
Architecture changes, crypto implementation, secrets handling outside synthetic
fixtures, exposed/public issuers, real PII and production remain outside this
admission.

## Mandatory source tests and blockers

Before an admission can become active, the exact approved head must pass all
applicable synthetic Docker checks, and unavailable checks must be explicitly
classified by the Owner against the concrete diff:

- Frozen install/build/typecheck, unit/contract/SQL migration first apply and
  replay, cross-DB and tenant/installation/workspace boundaries.
- For OIDC source: real provider Code + PKCE and verification-mail delivery;
  unverified/disabled account denial, state/nonce/PKCE and callback replay;
  issuer/audience/expiry/JWKS validation through maintained libraries.
  Dedicated issuer/audience/expiry/rotation test gaps are explicit deferred
  server-admission gates only for a closed synthetic prototype with no
  public/browser ingress; this deferral requires exact-record Owner review.
- Product Trust, schema/version, separate bounded entitlement, installation
  scope, grant/delegation ceilings; expiry, revoke, stale revisions, engine
  outage, pending projection, outbox replay and superseded-event denial.
- For lifecycle changes: wrong-person/wrong-organization denial, invite expiry
  and replay, no privilege promotion or reactivation after revocation.
- Restart and applicable pre-/post-revoke isolated recovery; an old DB snapshot
  cannot pass the current revocation epoch. A data-only drill is not a
  restored runtime/key/config acceptance or HA consistency proof.
- No published port, production Docker socket, privileged container, real
  credential/PII, hosted Actions or cross-product mutation. Human review of
  changed paths and targeted secret scan; no assertion of exhaustive absence
  of vulnerabilities from a pattern scan.

FAIL or UNKNOWN in a mandatory source test, unexplained privilege expansion,
unacknowledged critical/high finding, missing/expired approval, changed head,
missing protection or source-admission revision is STOP for source merge.
A deferred production gate is not PASS. Blocking findings require an actual
fix and exact-head regression evidence, never a blanket risk label.

The active record names its approver and exact SHA. A separate capability
authorizes each installation/credentials/deployment boundary; this source
admission does not create any of them. In-memory prototype sessions, fixture
sockets and recovery epochs are explicitly nonproduction, not approved
replacements for operational controls.

## Evidence statuses — independent, scoped facts

| Status | Required evidence |
| --- | --- |
| DEVELOPMENT_VERIFIED | Exact head and applicable Docker QA passed, known limitations recorded |
| SOURCE_MERGED | Actual remote main merge SHA under active source admission/review rules |
| SECURITY_ACCEPTED | Named independent qualified reviewer, exact scope/SHA and disposition |
| STAGING_VERIFIED | Separately admitted isolated server target and exact artifact verification |
| PRODUCTION_AUTHORIZED | Separate Owner and Infra approval for exact target/release |
| RUNNING_VERIFIED | Dated actual health/denial evidence for the named environment/artifact |

The statuses are not interchangeable or a blanket ordered ladder.
RUNNING_VERIFIED(local synthetic) is not production. SECURITY_ACCEPTED can never
be inferred from DEVELOPMENT_VERIFIED, a merge or the implementing agent's QA.

## Publication and targeted adoption

1. Owner reviews this ADR, affected clauses and Identity-only risk, including
   the proposed **MAJOR 4.0.0** classification and 14-day ceiling.
2. Reviewed source PR merges with exact immutable SHA. No normative status follows
   solely from this source merge.
3. A **separate reviewed release PR** publishes `standards-release.json` with the
   accepted version/source SHA, Owner approval, migration and impacted consumers.
   Preserve 3.0.0 history. No future SHA can be written in advance.
4. Identity adopts the published immutable pin in its own reviewed PR, reconciles
   acceptance/handoff and records the bounded admission. No automatic adoption
   by Website, Core, Infra or any other consumer.
5. Revalidate PR #4 then source-merge if admitted. Retarget PR #5, inspect the new
   diff and revalidate the resulting head and admission before its source merge.
6. Expiry, revocation, changed head, failed QA or critical/high finding closes the
   admission. Already merged source does not grant runtime permission. Revert
   unsafe source via reviewed PR when required; never delete evidence.

## Impact and acceptance

Affected consumer: Identity only. Core/Website/Reader/Bot/Planner/Gateway/
Infra/Automation keep their current review and release rules. The public
organization profile was considered: no product catalogue or runtime readiness
claim changes, so no profile edit is required for this proposal.

No hosted GitHub Actions, package publication, paid service, host resources,
signing keys, accounts, source merge permission or deployment is created here.
The proposed ADR and conditional links must pass docs-only QA. Source admission
and server/production acceptance stay distinct even after publication.

## Concrete pending chain, 2026-11-11

The current Identity chain is #4 → #5 → #6. Initial source heads are
`acc3ce73f9075ab121dad0deaf9b2709be95e5e5`,
`ecda01f73493d6995d03ed7bdb0ae34181f1725c`,
`e1196a25a6312ad9a30f9144807a461adf633347`.
These are **candidates, not approved admissions**. Retarget/merge-base changes
require diff review and final-head QA even when the head itself is unchanged.
Approved standard adoption can change these heads; any resulting new head must
replace its candidate only after actual testing and Owner approval.

[Pending admission records](../identity-source-admission-2026-11-11.md) contain
exact scope and a fixed expiry, never a future invented release SHA.
[GitHub protection runbook](../identity-main-protection.md) records the plan
limitation. This ADR explicitly does not waive it.
