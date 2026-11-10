# Pending Identity source admissions — 2026-11-11

**DRAFT / NOT AUTHORIZATION.** Owner supports the direction but has not approved
these final records under an effective published rule.
Normative 3.0.0 remains in force. No real data, deployment or security acceptance.

Common repository/target: E7A-RU/identity, protected main, source-only PR merge.
Proposed accountable approver/revocation owner: E7A Owner.
No author may manufacture the Owner's signature/review.
Requested fixed expiry for all three: **2026-11-25T00:00:00Z**.
Not-before: actual immutable 4.0.0 publication, targeted adoption, effective
protection and exact-record Owner approval, whichever is last. At approval
the remaining interval must not exceed 14 days. No retroactive validity.

| Record | Candidate exact head | Dependency | Scope |
| --- | --- | --- | --- |
| ID-SOURCE-4-20261111 | `acc3ce73f9075ab121dad0deaf9b2709be95e5e5` | current main | Foundation, Docker, domain/contracts, synthetic QA and migration 001 |
| ID-SOURCE-5-20261111 | `ecda01f73493d6995d03ed7bdb0ae34181f1725c` | #4 accepted, base main | Real synthetic OIDC/API, trust/entitlement/grant/outbox/recovery, migrations 002–005 |
| ID-SOURCE-6-20261111 | `e1196a25a6312ad9a30f9144807a461adf633347` | #5 accepted, base main | Person-bound membership invite/accept/revoke, no implicit owner/admin, migration 006 |

These heads cannot be approved if adoption changes their source. Preserve the
record history, list the new exact head, new diff and new QA receipt; do not
silently expand the original approval. Prefer ordinary merge commits to
preserve ancestry. A retargeted PR requires reviewed unique changed paths and
final-head Docker QA; a prior base-branch receipt is not enough.

## Current development receipts — not final merge receipts

| Record | Recorded commands/checks | Local evidence SHA-256 |
| --- | --- | --- |
| #4 | Docker build/typecheck, 32 unit, 4 vendor integration, SQL isolation/migration replay | `d73d18cb981badedde5c7d233f446d240ebf5243a10e03a7b2193790c8f3ccab` |
| #5 | Same foundation checks, real OIDC/email, trust/entitlement/grant/outage/outbox, restart, old/new snapshot recovery | `5826770f44d6b4fa3f258665e48a0cd0009edfb25eb712fca0524067a12fa72c` |
| #6 | Membership invite/accept/revoke and negative lifecycle checks; 32 unit/4 vendor checks, real vertical and old-backup regression | QA `e65e72b9a295cdca9da697000f4880aa48ba19f9941b8843115325ca802b8378`; regression `5478cea33267aba91d8b54b18e31310bf276db1d8f0317ed84a0198acd0c6580` |

All are local Docker Desktop arm64. Their logs are local, not independently
retained server attestations; exact commands and paths are in their PR bodies.
Re-run applicable acceptance before final admission and after source/base changes.

## Required dispositions before signing

- [ ] Normative source/publication SHAs actually exist; Identity adopted them.
- [ ] GitHub protection enabled and effective for main; no bypass or direct push.
- [ ] Concrete diff/known critical and high findings reviewed; none unremediated.
- [ ] Applicable mandatory tests all PASS, other source tests have reviewed
      NOT_REQUIRED rationale; no mandatory FAIL/UNKNOWN accepted.
- [ ] For #5/#6, dedicated negative issuer/audience/expiry/rotation and provider
      session propagation remain explicit server-admission blockers, not claimed
      PASS. Existing maintained-library validation and real code flow verified.
- [ ] Fixtures confined to local synthetic network, no exposed ports or real
      accounts/credentials. Current data/epoch recovery is not live vendor
      key/config acceptance.
- [ ] Active Owner approval binds exact head, admission ID, expiry and scope.
- [ ] No auto-merge, production, first-user, server host or package permission.

Statuses now: DEVELOPMENT_VERIFIED on recorded scopes; SOURCE_MERGED=false,
SECURITY_ACCEPTED=false, STAGING_VERIFIED=false, PRODUCTION_AUTHORIZED=false.
RUNNING_VERIFIED is only the dated local synthetic runtime, not production.
Independent security review remains required before real users, including pilots.
