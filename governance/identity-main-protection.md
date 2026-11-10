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
5. Require **one approving review** from an authorized human other than the PR
   author. Enable dismiss stale approvals and approval of the most recent
   reviewable push. Owner review can approve a source-only admission after
   publication; do not label it independent security acceptance.
6. If supported, restrict updates/allowed merge actors to the Owner or explicitly
   delegated maintainers while still enforcing PR/review gates. Do not put them
   in a bypass list. Confirm the permission with a no-change readback.
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

## Current consequence

Until real protection is available and read back, the source-admission ADR does
not permit merging #4/#5/#6. The Owner can choose a supported private plan or a
separately reviewed policy change; this run chooses neither on their behalf.
Other engineering preparation can proceed without changing repository exposure.

Official references:
[protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches),
[rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets).
