# Identity GitHub Free source-merge controls

**PROPOSAL · 2026-11-11 · active only after GOV-ID-SOURCE-001 publication/adoption.**

Observed: E7A-RU/identity is private; E7A-RU plan=free; main protected=false.
Admin GETs for branch protection and rulesets returned HTTP 403 requiring
an eligible plan or public repository. Connector admin capability alone cannot
remove that limitation. The Owner will not buy a plan or make Identity public.

This is an explicitly accepted organizational risk **only through the published
Identity exception**. Manual controls are not protected branches. GitHub may
still permit a writer to violate the procedure technically. Do not claim
technical prevention, emulate green checks, or weaken other repository rules.

## Before each permitted merge

1. Confirm the effective published Governance descriptor, Identity adoption
   and that the permanent source procedure has not been paused/revoked.
2. Read current writers and minimize access through separately authorized Owner
   administration. Do not revoke accounts or modify organization rules blindly.
3. Read remote main SHA, PR base/head, unique diff/paths, unresolved findings,
   actual review and Owner admission. The record must cover the exact current
   head, expected main and scope. No queued/automatic merge.
4. Require applicable Docker QA and negative cases on that exact head.
   No mandatory FAIL/UNKNOWN or known critical/high problem may pass.
5. Retain explicit attributable Owner approval. Same-login authors cannot create
   their own GitHub approving review; an exact-SHA Owner decision is recorded
   as such, never called independent security acceptance.
6. Coordinate a single merge window. Immediately re-read main and PR head.
   Unexpected drift, new comment/finding or changed base is STOP for reinspection.
7. Merge via GitHub PR using normal merge and the API expected-head SHA guard.
   No direct push, force push, rebase/squash rewrite or bypass of existing rules.
   GitHub's head guard does not lock the base; this residual race is disclosed.
8. Read the returned merge SHA and remote main. Verify the normal merge parents
   are the expected prior main and tested head, inspect its tree/changed paths
   and record result. Unexpected result: STOP, preserve evidence, investigate.
   Never reset/force main or automatically fabricate a repair.
9. Run applicable post-merge compatibility/QA; retain migrations and synthetic
   volumes. Retarget and revalidate the next stacked PR only afterward.

## Source admission record

Record PR, full source head, expected main/base, approved diff, applicable QA
commands/results/log hashes, known/deferred risks, attributable Owner decision,
actual merge SHA/parents/tree, post-merge outcome and closure/revocation.
No blanket future-code approval and no calendar expiry for the procedure.
Changed head/dependencies, unexpected main, pause/revoke, failed QA or a new
critical/high finding invalidates the relevant admission.

## Native settings when available — optional reassessment, not a purchase gate

Periodically assess whether native protection is needed/available, without any
obligation to buy it. If later authorized and supported: Identity → Settings →
Rules → Rulesets → new Active branch ruleset targeting main; require PR,
conversation resolution, block force/delete, no bypass actors; choose a feasible
native human review profile. Authors cannot approve their own PRs.
Classic protection with an Owner/maintainer update allowlist is an alternative.
Ruleset Restrict updates relies on bypass actors, so do not mistake it for a
classic push allowlist. No required hosted Actions, auto-merge or deployment.

Until publication, the existing 3.0.0/source acceptance remains binding. This
runbook is not itself an exception or a GitHub settings change.

Official availability:
[protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches),
[rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets).
