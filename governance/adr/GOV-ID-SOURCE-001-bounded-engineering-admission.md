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

## Proposed rule

Only **E7A-RU/identity**, local synthetic Docker engineering, may receive a
bounded **source-only admission** after this rule has become normative and
Identity has adopted its exact revision. It is not an R0/R1 auto-deploy or
ecosystem-wide exception.

Each admission is an Owner-reviewed record containing:

- Unique admission ID, product, repository, target branch, PR numbers and exact
  permitted head SHAs; finite expiry no longer than 30 days; revocation owner.
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

1. Owner reviews this ADR, affected clauses and Identity-only risk; settles
   release classification. A weakened mandatory security boundary may require
   MAJOR classification; do not silently label it a patch.
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
6. Expiry, revocation, changed head, failed QA or critical finding closes the
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
