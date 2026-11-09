# ADR proposal — GitHub Free private-repository release admission

**Status: PROPOSED / NOT APPROVED / NOT NORMATIVE.** Owner E7A must explicitly approve a Governance revision and Infra must independently verify activation. Current approved Governance 2.0.0 SHA `298fe2856f6ac879286f96d089175fa960c88dd8` remains unchanged. No product timer or standing authorization is enabled by this ADR.

## Decision requested

For **E7A Website only**, permit a strictly bounded, host-verified alternative to GitHub branch protection when the repository is private on GitHub Free and the owner explicitly declines a paid GitHub plan.

The existing protected-main route remains preferred. Alternative scope is only auto-deployment of **R1 content/locales/raster** changes, with *all* product-specific QA, immutable artifact, rollback, stop, owner authorization and Infra controls preserved. It must **never** permit automatic updates to dependencies, infrastructure, release scripts, security, IAM, databases or other R2+ changes.

### PR merge-attested SHA admission

Before executing any candidate QA or promotion:

1. Trusted root-controlled release controller obtains `old = last_successful_commit` and `new = origin/main`, and checks ancestry, clean checkout, exact SHAs and service identity.
2. Enumerate **every commit reachable on the first-parent path from old (exclusive) to new (inclusive)**; refuse empty/ambiguous/missing ancestry or excessive commit count. This covers intervening docs and dependencies, not only final commit or PR label.
3. Each commit **must** be the exact GitHub-API `merge_commit_sha` of a merged PR with `base.ref=main`, `base.repo.full_name=E7A-RU/website`, and `merged_by.login` on an independently owner-approved **allowlist** of GitHub login IDs. Reject deleted/inaccessible metadata, unknown branch associations, missing merge attribution, API error, multiple inconsistent matches and direct pushes. In squash/rebase cases accept only the exact SHA GitHub reports as the merged PR's result; rebase-associated ambiguous commits fail closed.
4. Pull-request API reads use an **independent read-only credential** stored outside Website checkout, never mounted into candidate builds. The token must be scoped to the single Website repository and only necessary read permissions. The candidate is not permitted to supply responses, override allowed reviewers, change API URL, or change controller code. Audit log stores SHA, PR number, merging account and evidence time, **not credentials**.
5. Existing strict file-diff classifier still decides R1 eligibility using the *aggregate* actual diff, including the currently implemented Git tree file mode check. A separately approved standard may also demand per-commit touched-path review to prevent malicious intermediate changes later reverted; until tested, the proposed acceptance MUST check full commit range paths and forbid any R2+ paths in intervening commits, even if final aggregate diff appears R1.
6. After QA, re-fetch `origin/main` and repeat exact SHA/branch/marker checks before promoting the **identical immutable Docker image ID**. On failed admission or API outage: **SKIP/BLOCK, no production modification**. Missing PR metadata or no matching allowed merge is NEVER treated as success.

### Threat model and limitations

This gate prevents an *ordinary uncontrolled direct push* to unprotected `main` from automatically deploying and creates an auditable allowlist of approved GitHub merger identities. It does **not** provide the server-side prevention of force pushes or direct pushes that GitHub Team branch protection does; a compromised approved GitHub account, compromised GitHub API/token trust or malicious signed-in merger remains in the threat model. A forced `main` rewrite must fail non-fast-forward checks; an attacker who can write to the Website server or root-level Docker daemon bypasses this source gate. Therefore Infra must close the privileged runner boundary and accept residual risks explicitly.

The proposal **does not mean that a test green mark approves source**. A PR-merge identity proof is only an admission signal; exact-SHA tests and runtime recovery are independent obligations. No second GitHub billing plan, GitHub Actions CI, new VM or mandatory staging is required.

### Activation/rollback and governance scope

- Separate Owner decision with bounded time, permitted GitHub merge account IDs, product, host, R1 path risk, expiry, revocation and alert path.
- Infra review of exact implemented host code, read-only token scopes, root-owned trust root, least-privileged QA and separated promotion.
- Negative suite: direct push, wrong merge account, wrong PR SHA, PR into wrong repo/branch, absent token, 403/rate limit, API timeouts, intervening R2 commits (even reverted), moving `main`, race and non-FF, candidate QA failure, rollback and RECOVERY_REQUIRED.
- Initial `AUTO_DEPLOY=DISABLED` and one controlled canary; active state only after first attested automatic R1 deployment plus health/rollback proof.
- Emergency stop must be independently operable; avoid replacing default protected-main requirement globally. This ADR proposes a **Website-specific exception**, not a universal waiver for E7A consumers.

## Dependencies and no-go gates

Production source currently `e44bc1d67986e9553b580dcc5f1ee1fa8bcfeec5`; GitHub Website `main` has newer documentation-only commits, but unattended Website deployment has never been enabled. Approved Governance 2.0.0 currently requires protected `main`; this ADR is not effective without a separately published normative change, which must update release descriptor/version/impact matrix through Owner change control. Merely merging this proposal **must not** alter `governance/standards-release.json` or any consumer pin.

**No approval is implied by owner's interest in automation or preference to stay on GitHub Free.**
