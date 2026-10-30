# E7A Sprint — independent Product Stewardship rollout, 2026-10-30

**Status:** scope and review ledger, **not** a production, F1/F2 or QA pass certificate. Governance source: [approved v1.1.0 release](standards-release.json), immutable normative SHA `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`. Product-specific agents operate under [Product Stewardship](product-stewardship.md).

## Цель и архитектурный предел

Дать каждому самостоятельному продукту единый проверяемый вход для ВП/агента: `README.md`, `AGENTS.md`, `governance-consumer.json` и `docs/MAINTAINERSHIP.md`. Сохранить раздельные владельцев данных, Kit, релизы, security boundaries и честные признаки готовности.

**Owner E7A** остаётся единственным текущим accountable normative и production approver. Назначения Product Owner/ВП/QA не придумываются. ВП ведёт свой продукт через Draft PR и локальную/самостоятельно одобренную self-hosted QA; без GitHub Actions CI.

## Закрытые в предыдущем проходе документы

- [Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1) — **MERGED docs**, each already pinned Governance 1.1.0.
- Website #57 squash `bc11b3fec7eaa62a92fa499aac1c534aa3dd6cfd` merged following owner Lobachevsky read-only output: user legacy cron **commented**, root/system cron checks for that script had no matching active line; Website checkout remained `e17bc8fc7cf6d07f8a8a13d189f29f56c2e86617` and container `e7a-web-1` healthy. **This is not a complete audit of every timer or a new deployment.**
- [Org Governance #8](https://github.com/E7A-RU/.github/pull/8) merged inventory reconciliation; [Bot #9](https://github.com/E7A-RU/bot/pull/9) merged Dependabot guard for unreviewed major Node typings.

## Stewardship rollout — intended verified source paths

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

**Important:** table states describe planned repository commits within this sprint; query linked PR and product `main` for actual post-merge status before reporting DONE. `governance-consumer.json` is only a **document pointer**, not implementation or production proof. Games is an approved direction with no confirmed code repository; it is not a missing consumer.

## Security and PR acceptance

- Document-only PR: review full changed-file list, links/JSON and existing file preservation; no service/Docker/package/lockfile/Actions changes; only merge with owner authorization and exact-head lease.
- Functional PR: frozen package dependencies and exact SHA tests + relevant negative/integration/browser/restore QA; do **not** mark Ready for review merely because Git is mergeable.
- Unmerged feature branches that contain README/AGENTS versions must be reconciled **without force-push** after stewardship docs hit main; preserve both high-sensitivity agent rules and current feature code.
- Dependabot PRs: lockfile/test/compatibility checks, no auto-merge or hosted Actions. No version bump without real QA.
- No production, WireGuard/Butlerov, server, DNS or other host modifications during sprint.

## Milestone exit criteria

- [ ] Main of each **initialized** repository contains README, AGENTS, `governance-consumer.json` and `docs/MAINTAINERSHIP.md`.
- [ ] `identity` and `platform` explicitly remain NOT IMPLEMENTED; no fake runtime claims.
- [ ] All doc-only stewardship PRs reviewed, paths valid and merged; actual commit SHAs recorded in GitHub.
- [ ] Planner/Gateway/Automation feature PRs remain safely ongoing and do not unknowingly revert adopted AGENTS/README/manifest.
- [ ] Any unresolved dependency/product PR has a named blocker, tests needed and responsible decision.
- [ ] Automation read-only inventory and future Draft PR propagation may use the new manifest, but permissions for real cross-repo writes remain disabled until independently approved.

[Product Stewardship](product-stewardship.md) · [Local handoff template](templates/product-maintainership.md) · [Product Lifecycle](product-lifecycle-checklists.md) · [Repository Impact Matrix](repository-impact-matrix.md).
