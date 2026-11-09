# E7A — сверка фактического состояния экосистемы

**Срез:** 31.10.2026. **Тип:** read-only GitHub source inventory + historical operator evidence; **не** сертификат QA, не онлайн-мониторинг, не разрешение release/deploy.  
**Normative baseline:** [Governance v1.1.0](standards-release.json), owner-approved, immutable SHA `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`, adopted on 30.10.2026. Наиболее свежий `main` того или иного репозитория **не** равен автоматически проверенному production SHA.

## 1. Иерархия ответственности — это уже не «четыре репозитория»

- **[Organization Governance](README.md):** нормативные решения, бренд, права и Product Stewardship; implementation specifics принадлежат продуктам.
- **[Core](https://github.com/E7A-RU/core):** reusable SDK, публичная UI Design System, нейтральные server contracts и необязательный Starter. **Нет центрального backend runtime, IAM, общей БД или продуктового deployment.**
- **[Laboratory](https://github.com/E7A-RU/laboratory):** Wizard / три Product Kits, QA evidence, release approvals, Developer Portal; текущее наличие документов **не** доказывает работающий сервис.
- **[Infra](https://github.com/E7A-RU/infra):** декларативная инфраструктура, запуск/health/rollback продуктов, контроль KS54 и Lobachevsky. Созданный репозиторий не подтверждает развёрнутый controller.
- **[Identity](https://github.com/E7A-RU/identity):** отдельные Person/Account/SecurityActor/SSO/Policy. **Documentation-only bootstrap**, работающего IdP не подтверждено.
- **[Platform](https://github.com/E7A-RU/platform):** **Delivery/Jobs/Connectors/Results**, не общее хранилище всей продуктовой логики. **Documentation-only bootstrap**; работающие jobs/коннекторы не подтверждены. Identity выделен **отдельно**, не модуль Platform.
- **[Automation](https://github.com/E7A-RU/automation):** private engineering automation / governance-sync, пока **bootstrap only**: GitHub App, install и write-capable service не подтверждены.
- **[Website](https://github.com/E7A-RU/website), [Reader](https://github.com/E7A-RU/reader), [Bot](https://github.com/E7A-RU/bot), [Planner](https://github.com/E7A-RU/planner), [Gateway](https://github.com/E7A-RU/gateway):** независимые продукты, каждый владеет своими данными, релизами и deploy. Reader локально обрабатывает документы без обязательного IdP; Bot владеет API, Postgres/RabbitMQ и Worker. Planner/Gateway не объявлять production-ready на основании репозитория.

Обязательная модель: [E7A Final Architectural Baseline](final-architecture-baseline-2026-10-27.md), [Product Stewardship](product-stewardship.md), [Automation](engineering-automation.md), [Backend Reference](backend-reference-architecture.md) и [Kit Roadmaps](kit-implementation-roadmaps.md). Старый [Core Platform RFC](https://github.com/E7A-RU/core/blob/main/docs/architecture/platform-backend-rfc.md) — **исторический проект** до выделения Identity и Platform в собственные репозитории; не использовать его как нынешнее решение.

## 2. GitHub `main` и открытая работа на момент среза

| Repo | Main SHA (31.10.2026) | Открытые PR | Проверенное состояние, не смешивать |
| --- | --- | --- | --- |
| [core](https://github.com/E7A-RU/core) | `027d7d3b52f632c92342048640da43a682c39869` | [#27](https://github.com/E7A-RU/core/pull/27) Draft — optional read-only product release CLI | Код SDK/UI; UI 0.2.1 публично опубликован; #27 **не** готов без exact-head QA |
| [website](https://github.com/E7A-RU/website) | `7d0f02055ab23d88a92d8054c4d62271c16ac3d6` | [#63](https://github.com/E7A-RU/website/pull/63) Draft — server-side QA gate, [#60](https://github.com/E7A-RU/website/pull/60) Next, [#61](https://github.com/E7A-RU/website/pull/61) React Dependabot | Последняя проверенная работающая прод-версия из [операционного журнала](https://github.com/E7A-RU/website/blob/main/docs/production-operations.md) — `e17bc8fc7cf6d07f8a8a13d189f29f56c2e86617` (25.10), не новый main |
| [reader](https://github.com/E7A-RU/reader) | `680fd3f5c6590c2affe39b213926e21cb0613f79` | [#8](https://github.com/E7A-RU/reader/pull/8) Next Dependabot | Browser-only MVP исходники; production не подтверждён; до внедрения UI 0.2.1 нет подтверждённой зависимости |
| [bot](https://github.com/E7A-RU/bot) | `9bc0ec8b28696c6c9b97a7ba6bd2aeadeeb60025` | [#7](https://github.com/E7A-RU/bot/pull/7) Studio framework, [#10](https://github.com/E7A-RU/bot/pull/10) Node types Dependabot | Bot API/worker и Studio — продуктовая логика; реальный свежий DB/MQ prod smoke **не проверялся** в рамках этого среза |
| [.github](https://github.com/E7A-RU/.github) | `c752b4999b07e1107ed39a321612c961e9527267` | нет на момент открытия этой ветки | Governance v1.1.0 и публичная Brand Policy в main |

Иные подтверждённые репозитории `main` (только source/ref, **не** runtime): [infra](https://github.com/E7A-RU/infra) `838a5e115560b09f32e24fdba1e2cb2d5c86dedf`; [laboratory](https://github.com/E7A-RU/laboratory) `07f0594adbf71d72d33c702c3b58d51aa7c2e335`; [identity](https://github.com/E7A-RU/identity) `224191c3eae1ada97afed2e7aa8aa2e1c218a585`; [platform](https://github.com/E7A-RU/platform) `699c097a7ace2c39ebe5944abfc5db6e3ad6ae74`; [planner](https://github.com/E7A-RU/planner) `b0aac9a8632fa151cd7bee151a7e67af0a422dbe`; [gateway](https://github.com/E7A-RU/gateway) `01963964e328b726f98c5bf36bc2ce45538db1a3`; [automation](https://github.com/E7A-RU/automation) `f46c334ded89694131262124cae62790358a41f6`.

## 3. Package / registry evidence

- `@e7a-ru/ui@0.2.1` published **publicly on npmjs.com**, authenticated publish and anonymous HTTP 200 metadata/readback with `dist.shasum=224404c2ffacfa0813d65dd0341979239885190a` ([Core #24](https://github.com/E7A-RU/core/issues/24)). At snapshot, Core main manifest also reads 0.2.1. This does not imply all products installed it.
- `@e7a-ru/ui@0.2.0` historical **private GitHub Packages** version; keep immutable history, do not delete a real release.
- `@e7a-ru/server@0.1.0` last confirmed **published private** GitHub Packages; `0.2.0` manifest exists in Core source, **not** confirmed published. No central API is created by the library.
- Website main declares public UI exact `0.2.1` while retaining independent app version `e7a@0.1.0`. Reader and Bot Studio current manifests **do not declare** `@e7a-ru/ui` at this check.
- Public npm artifact's `UNLICENSED` status and [Brand Policy](../BRAND_POLICY.md) remain separate: publicly readable is not freely reusable code or permitted use of E7A logo/mark without prior written permission.

## 4. Costs, security and release gates

- **No GitHub Actions CI**: main for Website, Reader, Bot and organization has no `.github/workflows` files; Core has only the **manual-only** `publish-versioned.yml` for authorized package publication. Dependabot-generated version PRs are **not** evidence that any tests ran and must not auto-merge/deploy.
- **Core release ≠ Website/Reader/Bot release**. The only historical Website production proof is in its dated operational journal, not continuous telemetry. `main` advancement or PR merge cannot assert production freshness.
- **One task → owned branch → Draft PR → exact-SHA external QA → human review → merge → separately owner-approved deployment**, without blanket `sudo`, system packages or other KS54 vhost changes. Laboratory release evidence and Infra deploy contracts are planned organizational roles, **not** assumed to be already running.
- Governance document propagation can eventually be proposed by self-hosted E7A Automation, but automation is **not active** just because its repo exists. No silent backfill, no automatic data access, no claims of a live orchestrator.

## 5. Detected documentation and evidence gaps; next actions

1. **Core `docs/status.md`** still describes October 25 and asks consumers to adopt public UI after a verified release; refresh its current source status and point to this portfolio snapshot without rewriting historical evidence. Core responsibility charter has an obsolete statement that it is still under PR review, while it is now merged.
2. **Reader `docs/status.md`** still says "not yet in Reader main" — accurate on current manifest, but does not mention Governance 1.1.0 and main SHA; verify Reader acceptance separately.
3. **Bot `docs/status.md`** is a historical October 25 table, not present operator attestation, and does not list current Governance 1.1.0 or Dependabot PRs.
4. **Website** has the most detailed operator runtime journal, but **no 31.10 host check** has been conducted in this task. Do not claim that `main` `7d0f020…` is deployed; confirm via read-only Lobachevsky checks before any release.
5. **Core #27, Website #63 and dependency PRs** need assigned reviewers and evidence on exact current heads before any merge. Avoid hosted GitHub CI, and do not treat docs correction as QA acceptance.
6. Cross-product backend boundaries now belong to separately established **Identity** and **Platform**, with **Infra/Laboratory** as other F1 foundations. Maintain Governance baseline, not the superseded Core shared-platform RFC.

**Evidence level:** source/documentation/PR inspection from connected GitHub on 31.10.2026, plus explicitly dated operator logs linked above. Not an npm live query or live server SSH check, no privileged mutation, no deployment or package publish.
