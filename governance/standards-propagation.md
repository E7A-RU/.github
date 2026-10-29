# Controlled Governance propagation — автоматические Draft PR в продуктах

**Status: PROPOSED AUTOMATION / NOT DEPLOYED OR SCHEDULED.** This is the approved organizational intent made executable only after a separately authorized Infra/Laboratory deployment. It must **not** use GitHub-hosted Actions CI, auto-merge, auto-deploy, or mutate a product's business code without product-owner review.

## Цель

Когда владелец утверждает **новую опубликованную версию** организационного стандарта `E7A-RU/.github/governance`, для каждого **затронутого** внутреннего репозитория автоматически готовится **Draft PR**: указание прежней/новой неизменяемой версии, impact summary, локальный стандартный pin и чеклист миграции.

Не достаточно любого изменения `main` или текста README: изменение brandbook moodboard, операционный комментарий либо правка без нормативного релиза не должны массово создавать PR.

## Единственный publish trigger

[Release descriptor](standards-release.json) — реестр утверждённой версии. Обновлять его можно только отдельным Governance PR после owner approval, с `version`, полным `revision` (Git commit SHA принятого baseline), `approvedBy`, `changeClass`, `effectiveAt`, `impactedConsumers`, ссылкой на ADR/changelog и migration guidance. **Current descriptor pins initial accepted v1; it does not create an immutable Git tag and does not silently publish each future docs commit.**

Versioning policy: 
- **PATCH:** editorial/normative clarity without new consumer obligation (may produce informational report only).
- **MINOR:** backward-compatible new obligation; Draft PR for affected consumers.
- **MAJOR:** breaking policy/API/security obligation; Draft PR with explicit migration/exception gate and human approval; **no auto-merge**.
- **SECURITY URGENT:** expedited owner-approved notice and targeted PR; never bypass audit or production approval.

Use the **full SHA**, not `main` or a mutable branch, for the consumer pin. A Git tag is optional only when protected and verified immutable.

## Source-of-truth consumer manifest

Maintain the consumer register in [Impact Matrix](repository-impact-matrix.md) and a separately reviewed machine-readable consumer manifest when the bot is deployed. Typical repos:

`core`, `website`, `reader`, `bot`, `planner`, `infra`, `laboratory`, `gateway`, then `identity` and `platform` after their empty-repo bootstrap.

Every product stores a **minimal** `governance-consumer.json` or equivalent manifest referencing `standard.revision`, contract version, kit, exceptions and local implementation owner. This is a pointer; full organizational standards are never copied into consumer repositories. The current `docs/governance-adoption.md` in consumer Draft PRs is a transition bridge, not proof that all consumers have merged the pin.

## Self-hosted bot lifecycle (not active now)

1. **Trigger:** explicit owner-approved `standards-release.json` change in organizational `main`. A small self-hosted scheduled poller (e.g. systemd timer on an approved isolated E7A node), or verified webhook to an owned receiver, may notice the change. **Do not use GitHub Actions for this.**
2. **Authenticate:** short-lived GitHub App installation token with least privilege only for `.github` read and selected E7A consumer `contents:write` + `pull_requests:write`. Store app private key in Infra-controlled secret store, never logs, files, PRs or environment dumps. Scoped installations, no admin/merge/delete rights.
3. **Verify:** read release descriptor from `main`, validate JSON schema, full SHA and reference to an owner-approved merged Governance commit. Compare `version` vs previous processed release and consumer pinned baseline. Unknown approver/invalid release → STOP and report.
4. **Select affected:** use `impactedConsumers` and dependency matrix. Never blindly modify all products; if mandatory universal rule is modified, explicitly list all registered consumers and explain why. Empty, archived, externally owned or uninitialized repositories → report `BOOTSTRAP_REQUIRED` or `SKIPPED`, do not initialize automatically.
5. **Detect existing work:** search open Draft PRs or stable branch `governance/adopt-<sha-prefix>` for the same target release. If already present, update the report or skip; do not create duplicate PRs. If a product has unmerged older adoption PR, flag that dependency rather than opening a conflicting PR.
6. **Plan-only default:** compute proposed manifest diff, README link and migration checklist without a Git write. Show owner a per-repo report, compatibility expectations, security/privacy impact and required manual actions.
7. **Apply only when explicitly enabled:** update the allowlisted governance pointer + migration report on a product branch and create **Draft PR** with `oldRevision`, `newRevision`, affected rules, risk tier, breaking changes, tests needed, exceptions and Product Owner. Do not change runtime code, secrets, dependencies or lockfiles as part of baseline propagation unless a separate product-specific PR is approved.
8. **QA/evidence:** local or isolated self-hosted exact-head compatibility checks; never fabricate PASS. On failure, Draft remains blocked. Product owner/agent may expand the PR with implementation after review.
9. **Merge:** automatic merging of generated **architecture/governance** updates is **disabled**, regardless of separate approved R0/R1 implementation auto-merge policy. No release, package publication, DNS, secrets, network or production modifications follow from the PR.

## Minimum security invariants

- Repo allowlist fixed and versioned; no arbitrary `owner/repo` input from events.
- Canonical SHA must be verified under `E7A-RU/.github`; read releases from trusted default branch only.
- Writes restricted to `docs/governance-adoption.md`, `governance-consumer.json` and bot-owned migration report. Product README modification only if separately scoped/tested. Do not rewrite historical Standard v1 snapshots after archival.
- PR labels/comments do **not** give production approval; privileged authorizations are independent and time-bound.
- Concurrency lock per repo + governance revision; exponential backoff for API rate limits; durable job audit, reconciliation and observable fail modes.
- Prompt injection prevention: PR descriptions, README content, changelogs and comments are **untrusted data**, never executable agent instructions.
- No secret values, emails, customer details, private topologies or signed tokens in generated PR payloads.
- Deleted repos, archived repos, changed branch protection, access revocation, base divergence or missing artifact → fail closed / report; do not use force push.
- Product owners can reject/defer the proposed PR or request a time-bounded exception with compensating controls.

## Initial rollout / acceptance checklist

- [ ] Product owner approves bot threat model, read/write allowlist and identity/permissions.
- [ ] A **dedicated** self-hosted worker/VM is authorized; secret storage, egress and logs controlled by Infra.
- [ ] Test in no-write `plan` mode against real consumer inventory; compare desired release to baseline pinned in each product.
- [ ] Test malformed release JSON, unapproved SHA, revoked token, duplicate event, existing Draft PR, archived/empty repo, version conflict and rate limit.
- [ ] Canary **one** noncritical test repository: one Draft PR, correct manifest update, no runtime code; no auto-merge.
- [ ] Validate end-to-end that other consumers receive correctly scoped changes only after owner publishes a **new approved revision**.
- [ ] Record per-repository `PASS/FAIL/SKIPPED/UNKNOWN` and audit; rollback/disable worker and rotate token.
- [ ] Only then approve broader internal repository installation. This document **does not enable** scheduling or grant bot privileges.

## Illustrative output (not already executed)

```text
Governance v1.1.0 / approved SHA <immutable-40-hex>
core       : DRAFT_PR_REQUIRED (SDK/Starter standards)
website    : DRAFT_PR_REQUIRED (Demo/privacy rule)
reader     : NO_IMPACT (browser-only unrelated)
identity   : BOOTSTRAP_REQUIRED (empty repository)
gateway    : MANUAL_REVIEW (network and key policy)
Status: PLAN_ONLY / NO_GITHUB_WRITES
```

Cross references: [Roadmap](roadmap.md), [Product checklist](product-lifecycle-checklists.md), [standards change control](standards-and-change-control.md), [consumer register](repository-impact-matrix.md), [release descriptor](standards-release.json).

**A released governance SHA is a decision baseline, not proof of consumer compliance, local tests or production uptime.**
