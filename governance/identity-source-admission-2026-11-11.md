# Pending Identity source admissions — 2026-11-11

**DRAFT / NOT AUTHORIZATION.** Owner supports the direction but has not approved
these final records under an effective published rule.
Normative 3.0.0 remains in force. No real data, deployment or security acceptance.

Common repository/target: E7A-RU/identity, main, source-only GitHub PR merge.
Proposed accountable approver/revocation owner: E7A Owner.
No author may manufacture the Owner's signature/review.
No calendar expiry or 14-day renewal. These records cover only their listed
PR/head and end on merge, Owner pause/revoke, changed head/dependencies/base,
failed mandatory QA or a new critical/high finding. No blanket future commits.
Not-before: actual 4.0.0 publication, controlled Identity adoption and the
specific attributable Owner decision, whichever is last. No retroactive validity.
The manual GitHub Free risk must be accepted in the published rule and each
record; technical protection stays UNAVAILABLE, not PASS.

| Record | Candidate exact head | Dependency | Scope |
| --- | --- | --- | --- |
| ID-SOURCE-4-20261111 | `acc3ce73f9075ab121dad0deaf9b2709be95e5e5` | current main | Foundation, Docker, domain/contracts, synthetic QA and migration 001 |
| ID-SOURCE-5-20261111 | `ecda01f73493d6995d03ed7bdb0ae34181f1725c` | #4 accepted, base main | Real synthetic OIDC/API, trust/entitlement/grant/outbox/recovery, migrations 002–005 |
| ID-SOURCE-6-20261111 | `e1196a25a6312ad9a30f9144807a461adf633347` | #5 accepted, base main | Person-bound membership invite/accept/revoke, no implicit owner/admin, migration 006 |

These heads cannot be approved if adoption changes their source. Preserve the
record history, list the new exact head, expected main, new diff and new QA receipt; do not
silently expand the original approval. Prefer ordinary merge commits to
preserve ancestry. A retargeted PR requires reviewed unique changed paths and
final-head Docker QA; a prior base-branch receipt is not enough.

Initial dependency snapshot: Identity main
`db33c47119a75974892fff8d5272f77f15b10ab9`; #4 targets main, #5 targets
the #4 branch, #6 targets the #5 branch. These are not approved final main/base
values after publication/adoption or prior merges.

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
- [ ] Published Identity-only GitHub Free exception applies; technical protection
      UNAVAILABLE is disclosed. Minimum writers and PR-only/manual history checks
      are confirmed; no existing GitHub/organization restriction bypassed.
- [ ] Current expected main/base SHA recorded immediately before merge; normal
      merge parents, resulting remote main/tree and post-merge QA will be checked.
- [ ] Concrete diff/known critical and high findings reviewed; none unremediated.
- [ ] Applicable mandatory tests all PASS, other source tests have reviewed
      NOT_REQUIRED rationale; no mandatory FAIL/UNKNOWN accepted.
- [ ] For #5/#6, dedicated negative issuer/audience/expiry/rotation and provider
      session propagation remain explicit server-admission blockers, not claimed
      PASS. Existing maintained-library validation and real code flow verified.
- [ ] Fixtures confined to local synthetic network, no exposed ports or real
      accounts/credentials. Current data/epoch recovery is not live vendor
      key/config acceptance.
- [ ] Active attributable Owner approval binds exact head, expected main, admission ID, scope and termination conditions.
- [ ] No auto-merge, production, first-user, server host or package permission.

Statuses now: DEVELOPMENT_VERIFIED on recorded scopes; SOURCE_MERGED=false,
SECURITY_ACCEPTED=false, STAGING_VERIFIED=false, PRODUCTION_AUTHORIZED=false.
RUNNING_VERIFIED is only the dated local synthetic runtime, not production.
Independent security review remains required before real users, including pilots.
