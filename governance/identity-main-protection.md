# Identity main protection — concrete setup, 2026-11-11

**PLAN ONLY; no repository settings were changed.**
Readback: E7A-RU/identity is private, main protected=false, E7A-RU plan=free.
Both branch-protection and ruleset GETs with repository admin rights return
HTTP 403: feature requires an upgrade or a public repository.
Lack of connector scope is therefore not the only limitation.

For private organization repositories, GitHub requires an eligible organization
plan such as Team/Enterprise. Do not make this repository public, buy a plan,
add bypasses or weaken Governance without separate explicit Owner authority.
Owner supports source-admission direction, not a plan purchase or public release.

## UI steps once the feature is available

1. Open Identity → **Settings → Rules → Rulesets → New ruleset → New branch
   ruleset**. Name: `identity-main-reviewed-source`; Enforcement: **Active**.
2. Target **Default branch**, or exact `refs/heads/main` if the UI requires it.
   Do not match the independent working branches.
3. Bypass list: **empty**, including administrators and apps.
4. Enable **Restrict deletions**, **Block force pushes**, **Require a pull
   request before merging**, **Require conversation resolution**.
5. Default: require **one approving review** from an authorized human other
   than the PR author; dismiss stale approvals and require approval of the
   latest reviewable push. This review need not claim independent security
   acceptance. GitHub forbids authors approving their own PRs. If Codex and
   Owner act through the same GitHub login, a different account is needed.
6. Do not enable the ruleset's **Restrict updates** merely to limit merge actors:
   it uses the bypass list and is not the same as a classic push allowlist.
   If merge-actor restriction is needed, use a **classic branch-protection**
   rule with `Restrict who can push to matching branches`, allow only the
   Owner/delegated maintainers, and keep all PR/no-bypass requirements.

7. Do not require GitHub Actions checks, workflows or deployment environments.
   Docker evidence belongs to the exact-head review/admission. Do not create
   fake green status contexts. Do not enable auto-merge or merge queue.
8. Repository Settings → General → Pull Requests: allow **merge commits**;
   keep auto-merge off. Use normal merge to preserve #4 → #5 → #6 ancestry.
   No direct push or squash that silently invalidates the tested chain.
9. Save, read the active ruleset/branch protection back, and verify that direct
   pushes, forced updates and deletion are denied. Record rule ID, effective
   target, required review count and administrator coverage before any merge.

A classic branch-protection rule is an alternative with the same requirements:
main pattern; require PR/review=1; dismiss stale reviews; latest-push approval;
resolve conversations; include administrators/no bypass; forbid force/delete;
restricted update actors where supported. Do not enable linear history if
ordinary merge commits are the selected chain-preserving method.

## Explicit solo-source alternative — proposal, not selected

The Owner may explicitly approve a **zero-native-approvals** source profile in
GOV-ID-SOURCE-001 publication/admission when the same GitHub account authors and
accepts the engineering PR. This is a disclosed manual Owner acceptance, never
an independent review. It is not inferred from the current direction approval.

Under that profile: PR-only remains mandatory; the classic push/merge allowlist
is Owner/delegated maintainer only; no administrator bypass, force push, delete,
auto-merge or direct main push. Required approving review count is 0 and the
latest-push approval option is off, because GitHub cannot enforce same-account
approval. The exact-SHA Owner admission, QA, expiry and blockers are mandatory
policy checks performed before merging; GitHub does **not** enforce their
semantics. The record must state this limitation. Any pre-existing stricter
rule remains binding until explicitly reviewed and changed by the Owner.
Never silently select this alternative to avoid a requested review.

## Current consequence

Until real protection is available and read back, the source-admission ADR does
not permit merging #4/#5/#6. The Owner can choose a supported private plan or a
separately reviewed policy change; this run chooses neither on their behalf.
Other engineering preparation can proceed without changing repository exposure.

Official references:
[protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches),
[rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets).
