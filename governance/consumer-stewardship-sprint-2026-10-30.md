# E7A Sprint — independent Product Stewardship rollout, 2026-10-30

**Status:** scope and review ledger, **not** a production, F1/F2 or QA pass certificate. Governance source: [approved v1.1.0 release](standards-release.json), immutable normative SHA `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`. Product-specific agents operate under [Product Stewardship](product-stewardship.md).

## Цель и архитектурный предел

Дать каждому самостоятельному продукту единый проверяемый вход для ВП/агента: `README.md`, `AGENTS.md`, `governance-consumer.json` и `docs/MAINTAINERSHIP.md`. Сохранить раздельные владельцев данных, Kit, релизы, security boundaries и честные признаки готовности.

**Owner E7A** остаётся единственным текущим accountable normative и production approver. Назначения Product Owner/ВП/QA не придумываются. ВП ведёт свой продукт через Draft PR и локальную/самостоятельно одобренную self-hosted QA; без GitHub Actions CI.

## Закрытые в предыдущем проходе документы

- [Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1) — **MERGED docs**, each already pinned Governance 1.1.0.
- Website #57 squash `bc11b3fec7eaa62a92fa499aac1c534aa3dd6cfd` merged following owner Lobachevsky read-only output: user legacy cron **commented**, root/system cron checks for that script had no matching active line; Website checkout remained `e17bc8fc7cf6d07f8a8a13d189f29f56c2e86617` and container `e7a-web-1` healthy. **This is not a complete audit of every timer or a new deployment.**
- [Org Governance #8](https://github.com/E7A-RU/.github/pull/8) merged inventory reconciliation; [Bot #9](https://github.com/E7A-RU/bot/pull/9) merged Dependabot guard for unreviewed major Node typings.

## Stewardship rollout — product snapshots at sprint start

| Repository | Existing source posture | Stewardship PR scope | Runtime or merge gate remaining |
| --- | --- | --- | --- |
| [core](https://github.com/E7A-RU/core) | SDK/packages and UI source, separate versions | README link + `docs/MAINTAINERSHIP.md` | [Core productctl #27](https://github.com/E7A-RU/core/pull/27) source QA before feature merge |
| [website](https://github.com/E7A-RU/website) | Dated Website runtime evidence; source main advanced independently | README link + `docs/MAINTAINERSHIP.md` | Dependabot [#60](https://github.com/E7A-RU/website/pull/60)/[#61](https://github.com/E7A-RU/website/pull/61), exact tests + separate production approval |
| [reader](https://github.com/E7A-RU/reader) | Browser-only/local-first | README link + `docs/MAINTAINERSHIP.md` | [Next patch #8](https://github.com/E7A-RU/reader/pull/8) with browser/privacy QA |
| [bot](https://github.com/E7A-RU/bot) | Bot owns existing API/DB/MQ/workers | README link + `docs/MAINTAINERSHIP.md` | Studio Dependabot requires exact-head QA |
| [infra](https://github.com/E7A-RU/infra) | Architecture only for shared control plane | README link + AGENTS + `docs/MAINTAINERSHIP.md` | QA VM, host/recovery remain unverified |
| [laboratory](https://github.com/E7A-RU/laboratory) | Product Factory charter, runtime not verified | README link + AGENTS + `docs/MAINTAINERSHIP.md` | Wizard/QA/Release Gate not deployed |
| [planner](https://github.com/E7A-RU/planner) | README-only main; [feature Draft #1](https://github.com/E7A-RU/planner/pull/1) contains design/Starter | README link, AGENTS baseline, manifest, `docs/MAINTAINERSHIP.md` | MVP scope and exact-head code QA pending; reconcile feature branch after docs merge |
| [gateway](https://github.com/E7A-RU/gateway) | README-only main; [feature Draft #1](https://github.com/E7A-RU/gateway/pull/1) contains PostgreSQL/TypeScript sources | README link, AGENTS baseline, manifest, `docs/MAINTAINERSHIP.md` | MFA/helper/network security tests and separate owner network approval; reconcile feature branch |
| [automation](https://github.com/E7A-RU/automation) | README-only main; [feature Draft #1](https://github.com/E7A-RU/automation/pull/1) PLAN_ONLY source | README link, AGENTS baseline, manifest, `docs/MAINTAINERSHIP.md` | Compiler lockfile/exact-head QA; App registration separately approved; reconcile feature branch |
| [identity](https://github.com/E7A-RU/identity) | Documentation-only README bootstrap, no IdP | README+AGENTS+manifest+`docs/MAINTAINERSHIP.md` | Separate F0/F1 charter, privacy and IdP ADR; no deployed users/IdP |
| [platform](https://github.com/E7A-RU/platform) | Documentation-only README bootstrap, no Delivery/Jobs runtime | README+AGENTS+manifest+`docs/MAINTAINERSHIP.md` | Separate F0/F1 contracts, provider readiness; no deployed connectors |

**Important:** the table above describes the *pre-merge state* at sprint start. Actual verified post-merge SHAs are recorded in the closeout below. Product `main` changes do not certify production runtime. `governance-consumer.json` is only a **document pointer**, not implementation or production proof. Games is an approved direction with no confirmed code repository; it is not a missing consumer.

## Security and PR acceptance

- Document-only PR: review full changed-file list, links/JSON and existing file preservation; no service/Docker/package/lockfile/Actions changes; only merge with owner authorization and exact-head lease.
- Functional PR: frozen package dependencies and exact SHA tests + relevant negative/integration/browser/restore QA; do **not** mark Ready for review merely because Git is mergeable.
- Unmerged feature branches that contain README/AGENTS versions must be reconciled **without force-push** after stewardship docs hit main; preserve both high-sensitivity agent rules and current feature code.
- Dependabot PRs: lockfile/test/compatibility checks, no auto-merge or hosted Actions. No version bump without real QA.
- No production, WireGuard/Butlerov, server, DNS or other host modifications during sprint.

## Milestone exit criteria

- [x] Main of all **11 existing repositories** contains README, AGENTS, `governance-consumer.json` and `docs/MAINTAINERSHIP.md`; verified from live Git trees and manifest readback on 2026-10-30.
- [x] `identity` and `platform` initialized **documentation only**; README, AGENTS and manifest explicitly say NOT IMPLEMENTED / NOT RUNNING.
- [x] All 11 doc-only stewardship PRs reviewed (relative links/manifest/diff), merged with expected head SHA lease; exact squash SHAs in closeout below.
- [x] Planner/Gateway/Automation feature PRs retain Draft status, feature source unchanged, and were reconciled with the new main via a two-parent no-force merge, including adopted AGENTS/README/manifest.
- [ ] Any unresolved dependency/product PR has a named blocker, tests needed and responsible decision.
- [x] New manifests are available for future read-only Automation; **actual private inventory/App write token/timer are not implemented or approved**, remain separate A2–A5 gates.

[Product Stewardship](product-stewardship.md) · [Local handoff template](templates/product-maintainership.md) · [Product Lifecycle](product-lifecycle-checklists.md) · [Repository Impact Matrix](repository-impact-matrix.md).


## Sprint closeout — GitHub-verified `main` (2026-10-30)

**Result:** 11/11 repository `main` branches contain all four required stewardship entrypoints, each pinning normative release **Governance v1.1.0** at `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`. Existing GitHub-hosted Actions exception remains only Core manual package publisher; **no new workflows** were introduced. These are documentation merges, not product tests/releases/deployments.

| Repository | Stewardship PR | Squash commit now in main | Source assertion |
| --- | --- | --- | --- |
| [core](https://github.com/E7A-RU/core) | [#28](https://github.com/E7A-RU/core/pull/28) | `027d7d3b52f632c92342048640da43a682c39869` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [website](https://github.com/E7A-RU/website) | [#62](https://github.com/E7A-RU/website/pull/62) | `7d0f02055ab23d88a92d8054c4d62271c16ac3d6` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [reader](https://github.com/E7A-RU/reader) | [#9](https://github.com/E7A-RU/reader/pull/9) | `680fd3f5c6590c2affe39b213926e21cb0613f79` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [bot](https://github.com/E7A-RU/bot) | [#11](https://github.com/E7A-RU/bot/pull/11) | `9bc0ec8b28696c6c9b97a7ba6bd2aeadeeb60025` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [infra](https://github.com/E7A-RU/infra) | [#2](https://github.com/E7A-RU/infra/pull/2) | `838a5e115560b09f32e24fdba1e2cb2d5c86dedf` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [laboratory](https://github.com/E7A-RU/laboratory) | [#2](https://github.com/E7A-RU/laboratory/pull/2) | `07f0594adbf71d72d33c702c3b58d51aa7c2e335` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [planner](https://github.com/E7A-RU/planner) | [#2](https://github.com/E7A-RU/planner/pull/2) | `b0aac9a8632fa151cd7bee151a7e67af0a422dbe` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [gateway](https://github.com/E7A-RU/gateway) | [#2](https://github.com/E7A-RU/gateway/pull/2) | `01963964e328b726f98c5bf36bc2ce45538db1a3` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [automation](https://github.com/E7A-RU/automation) | [#2](https://github.com/E7A-RU/automation/pull/2) | `f46c334ded89694131262124cae62790358a41f6` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [identity](https://github.com/E7A-RU/identity) | [#1](https://github.com/E7A-RU/identity/pull/1) | `224191c3eae1ada97afed2e7aa8aa2e1c218a585` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |
| [platform](https://github.com/E7A-RU/platform) | [#1](https://github.com/E7A-RU/platform/pull/1) | `699c097a7ace2c39ebe5944abfc5db6e3ad6ae74` | **README + AGENTS + MAINTAINERSHIP + v1.1.0 pin**; source-only stewardship |

[Organization Governance PR #9](https://github.com/E7A-RU/.github/pull/9) merged owner-directed Product Stewardship, template and sprint ledger at `a8428033a229316af306275971bcacd767f6698e`. [Website document adoption PR #57](https://github.com/E7A-RU/website/pull/57) separately merged at `bc11b3fec7eaa62a92fa499aac1c534aa3dd6cfd` after a dated owner operator check of the paused legacy cron; **no Website production redeploy**.

**Active Draft feature branch reconciliation (no force and no source-code changes):** [Planner #1](https://github.com/E7A-RU/planner/pull/1) head `7d2e10a5459cdf7f97145dee423dff2f9eca756a`; [Gateway #1](https://github.com/E7A-RU/gateway/pull/1) `2274abd16f586bff119597e455f761b9853f63b7`; [Automation #1](https://github.com/E7A-RU/automation/pull/1) `259a373800e575fbf3399eb2feeae71726c55278`. All were 0 behind main immediately after reconciliation, but **not accepted for functional merge**, and these SHAs can change as product agents continue development.

### Next owner/product-agent sprint (not covered by docs merge)

- [ ] Core productctl [#27](https://github.com/E7A-RU/core/pull/27): exact-head local tests + Starter integration, package version/release gates.
- [ ] Automation [#1](https://github.com/E7A-RU/automation/pull/1): pin reviewed TypeScript toolchain, exact-head tests, real authorized read-only GitHub inventory and release-owner-proof ADR; App write canary separately gated.
- [ ] Planner [#1](https://github.com/E7A-RU/planner/pull/1): Product Owner decides MVP scope/acceptance, then frozen install/build QA before functional merge.
- [ ] Gateway [#1](https://github.com/E7A-RU/gateway/pull/1): lockfile, PostgreSQL concurrent tests, strong auth/MFA, privileged helper, real network plan and independent owner approval; no WireGuard/Butlerov modifications.
- [ ] Reader/Website/Bot Dependabot: product-owner exact-head frozen install, typecheck, tests/build, browser/privacy/provider-specific regressions, rollback evidence; no automatic merge and **no hosted Actions CI**.
- [ ] Assign real Product Owner/VP/QA identities or record current one-human exception in local maintainership. The repository standard alone **does not appoint people**.

**Evidence limit:** product-local docs and manifests passed static/Git tree readback; no exact-head executable QA for the above open functional/dependency PRs was conducted in this documentation sprint. GitHub source merge is not deployed runtime.
