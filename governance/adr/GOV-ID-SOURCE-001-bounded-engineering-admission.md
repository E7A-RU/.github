# GOV-ID-SOURCE-001 — revocable Owner-controlled Identity source admission

**PROPOSED · Owner direction 2026-11-11 · NOT NORMATIVE UNTIL PUBLICATION.**

Owner requires GitHub Free and a private Identity repository, with no paid plan
or public exposure. The procedure has no calendar sunset. Governance 3.0.0
remains effective at `80b574cf01d2976b4112e3e492692b02233b9f44` until the
separate 4.0.0 publication and targeted adoption below. This source PR itself
does not activate the exception or approve any engineering PR.

## Class and exact exception

**MAJOR 4.0.0.** This changes mandatory security-review timing and the source
acceptance contract for **E7A-RU/identity only**. It is not a backward-compatible
clarification or permission for other products.

After publication/adoption, isolated synthetic Identity engineering may be
source-merged without prior independent security acceptance and without native
branch protection unavailable on the current private GitHub Free repository,
but only through the concrete Owner-controlled PR procedure below.
These are the **only** exceptions. Other applicable 3.0.0 architecture, source
QA, audit, privacy, release and runtime rules remain binding. No release/deploy
standing authorization, auto-merge, CI, product migration or server admission
is created.

## Duration and revocation

The procedure remains available until the Owner pauses, revokes or replaces it
through a recorded normative decision. There is no 14-day renewal, fixed
November deadline or automatic extension of individual admissions.
Each admission covers **one PR and one full tested head SHA**. It ends on merge,
Owner revocation/pause, changed code/dependencies/head, unexpected base/main,
failed mandatory QA, or a newly identified critical/high security problem.
The same admission never covers future commits. Updating an admission requires
new evidence and human Owner approval; the implementation agent cannot invent
that approval. A normative change requires reassessment of outstanding records.

## Scope and compensating controls on GitHub Free

Only local isolated Docker projects, synthetic users and test-only credentials.
No real PII, real signing/deployment credentials, public IdP/ingress, production,
existing product cutover or E7A host resource changes.
No arbitrary authorization expansion or own OAuth/WebAuthn/cryptographic stack.

GitHub reports Identity private, organization plan Free and main protected=false;
admin readbacks of branch-protection/rulesets return feature-unavailable 403.
**Manual controls are not technically equivalent to protected main.**
The Owner selects this organizational risk in the published rule. Direct or
forced writes remain technically possible for writers, even though forbidden
by this procedure. No status may call the technical protection gate PASS.

Compensating controls:
- Minimum necessary repository writers; Owner reviews access and revokes only
  through separately authorized access management. No automated access changes.
- Every source change, including Governance adoption, goes through a PR.
  No direct main write, force push, history deletion or PR bypass.
- Review exact diff, current remote main, current PR base/head and outstanding
  findings immediately before merge; no concurrent merge window.
- Bind Docker receipts and Owner approval to the full head SHA and expected
  base main SHA. A base change requires reinspection and revalidation, not
  reliance on an old base-branch receipt.
- Merge explicitly through GitHub's PR API/UI using normal merge commits and
  the expected-head guard. Never auto-merge or run a direct git push to main.
- Read remote main and the resulting merge parents/tree immediately afterward.
  Record actual merge SHA and verify expected ancestry/unique changed paths.
  The head guard is not an atomic base lock; an unexpected main/parent/tree
  is STOP and manual investigation, never automatic history repair.
- Periodically reassess native protection availability/need without an obligation
  to purchase it. Keep no-bypass native rules if any later become applicable.
  Existing stricter GitHub/organization rules are never bypassed.

See [concrete operator procedure](../identity-main-protection.md).

## Per-PR Owner admission

Required record: admission ID; E7A-RU/identity; PR; exact tested head; expected
main/base; changed-path/scope; known/deferred risks; Docker QA commands, exit
codes and log hashes; accountable human Owner approval/evidence; termination
conditions above. A template or direction approval is not its signature.

GitHub does not let an author approve their own PR. If Codex and Owner share
the same login, Owner acceptance must be retained as an explicit exact-SHA
decision/comment or other attributable Owner instruction, not a fabricated
GitHub APPROVED review. This is not independent security review.
The agent may merge only after all active-record requirements actually hold.
Critical/high authorization bypass, tenant escape, credential exposure, unsafe
data destruction, mandatory FAIL/UNKNOWN or rejected human review blocks merge.

Mandatory applicable Docker checks:
- Genuine frozen install, pinned images, build/typecheck, unit/integration;
  SQL first apply/replay, least database privileges and cross-tenant/DB isolation.
- Real OIDC Code+PKCE, synthetic email verification, disabled/unverified account
  denial, state/nonce/PKCE and callback replay; maintained issuer/audience/
  expiry/JWKS validation. Explicit negative issuer/audience/expiry/rotation and
  provider-session propagation gaps may be deferred **only** to server/user
  admission for a closed local prototype, with exact-record Owner disposition.
- Product Trust/schema/version, separate entitlement, expiry/revoke, installation
  scope, grants/delegation ceilings; engine outage, stale revision/projection,
  outbox retries/superseded events and no resurrection of revoked authority.
- For lifecycle changes: wrong person/org, invite expiry/replay, no privilege
  promotion or implicit reactivation after revoke.
- Restart and applicable old/new backup recovery; retained epoch must reject
  stale restored authority. Data-only restore is not a live key/config drill.
- Changed-path/diff review and targeted secret inspection; no hosted Actions,
  public ports, production socket, privilege or cross-product mutation.

A deferred gate is NOT_RUN/NOT_REQUIRED-with-reviewed-reason, never PASS.
No critical/high finding can be waived by calling the runtime synthetic.

## Evidence and server/user boundary

| Status | Meaning |
| --- | --- |
| DEVELOPMENT_VERIFIED | Exact head and applicable Docker QA passed |
| SOURCE_MERGED | Actual accepted remote main merge SHA |
| SECURITY_ACCEPTED | Independent qualified review, named scope/SHA and disposition |
| STAGING_VERIFIED | Separately admitted isolated server artifact/target verified |
| PRODUCTION_AUTHORIZED | Separate Owner/Infra release approval |
| RUNNING_VERIFIED | Dated runtime proof scoped to environment/artifact |

Local RUNNING_VERIFIED and source merge imply none of the later acceptances.
Before real users, including server pilots: independent security review, Infra
isolation, browser TLS/secure cookies, MFA/passkeys/recovery, full OIDC/JWKS/
session revocation, keys/config/DB and pre-revoke restore, reconciliation,
privacy/residency, resource/rollback proof and separate runtime/release authority.

## Publication and adoption

1. Revise source PR #26, inspect normative diff/impact and obtain explicit Owner
   approval of the concrete final source and MAJOR 4.0.0 classification.
2. Merge source under current applicable Governance docs/source gates. Capture
   actual accepted immutable SHA; source merge alone is not a normative release.
3. Publish a separate reviewed `standards-release.json` 4.0.0 PR pinned to that
   accepted SHA; retain 3.0.0 history, impact only Identity, and read back release.
4. Perform targeted Identity adoption through a controlled PR change. Do not
   use the unpublished proposal to approve that change. Record publication,
   adopted source and the one-time Owner adoption authorization.
5. Revalidate exact heads/admissions and merge #4 → retarget/revalidate #5 →
   retarget/revalidate #6. Record remote main after each. No PR #7 beforehand.

[Candidate admission records](../identity-source-admission-2026-11-11.md) are
unapproved templates until filled and signed under the effective rule.
Other products/pins and public runtime claims are unchanged. The organization
profile was reviewed for impact; this source-only proposal changes no catalogue
or readiness claim, so no public profile edit is needed.
