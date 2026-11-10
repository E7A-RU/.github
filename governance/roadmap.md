# E7A Roadmap — от Governance к работающим продуктам

**Статус:** OWNER-APPROVED STRATEGY / PROPOSED DELIVERY PLAN (2026-10-30). Канонический опубликованный baseline: [Governance v2.0.0 at immutable SHA](https://github.com/E7A-RU/.github/tree/298fe2856f6ac879286f96d089175fa960c88dd8/governance), [release descriptor](standards-release.json). Ранее принятый v1.1.0 baseline и исторические adoption evidence сохраняются; consumers переходят на v2.0.0 отдельными reviewed PR. Ниже перечислены **целевые критерии**, а не выполненные этапы, бюджет, обещания сроков или полномочия на production.

**Рекомендованная техническая база:** [Backend TypeScript / PostgreSQL-first](backend-reference-architecture.md), [эталонный roadmap реализации трёх Kit](kit-implementation-roadmaps.md), [ресурсы и расходы Github App](github-app-cost-and-resources.md).

**Практические шаблоны:** [Product Charter](templates/product-charter.md) → [checklists](product-lifecycle-checklists.md) → [Release Gate](templates/release-gate.md).

## Главные цели и инварианты

> **Governance 3.0.0 / Decision 064:** централизованное владение экосистемными правами и схемами в Identity, продуктовые данные и API enforcement остаются у продуктов. Исторические этапы этой Roadmap не подтверждают реализацию. См. [ADR](adr/GOV-AUTHZ-001-centralized-authorization.md). Публикация стандарта и adoption каждого продукта — отдельные контролируемые действия.


- **B2C-first**, B2B автоматизация — самостоятельное второе коммерческое направление; Website развивается параллельно Foundation.
- **Независимые продукты:** собственный домен, данные, применение авторизации и бизнес-инварианты внутри продукта, версии и релизы (экосистемные grants после Governance 3.0.0 принадлежат Identity). Общие сервисы подключаются по необходимости; анонимные Website/Reader/игры не требуют постоянного общего backend.
- **Foundation** — инженерная программа, а **не** обязательный единый репозиторий, монолит, уже выпущенная версия ПО или новый продукт.
- **Безопасность и правдивый статус:** APPROVED → IMPLEMENTED SOURCE → TESTED → RELEASE ELIGIBLE → RUNNING VERIFIED являются разными доказательствами; staging — optional risk evidence. GitHub Actions hosted CI запрещён, отдельная разрешённая ручная публикация пакетов Core — исключение. Production требует действующего scoped standing authorization либо отдельного owner approval; см. [release/deploy policy](release-deployment-policy.md).
- **Любая техническая веха проходит через [Product Lifecycle & Checklists](product-lifecycle-checklists.md).** Не смешивать governance adoption PR с релизом продукта.

## Портфель и этапы

| Контур / репозиторий | Целевой результат | Сегодняшний доказательный статус / последовательность |
| --- | --- | --- |
| [Governance](README.md) / .github | Утверждённая версия правил и controlled change management | Первичный baseline принят; consumer adoption и синхронизация ещё проходят PR |
| [Core](https://github.com/E7A-RU/core) | Версионированные библиотеки и опциональный Product Starter | Репозиторий существует, источник UI и server contracts; каждый новый выпуск отдельно проверяется |
| [Infra](https://github.com/E7A-RU/infra) | Безопасные ресурсы, self-hosted QA, секреты, backup/restore, risk-triggered staging, controlled auto/apply, node protocol | Целевая концепция / Draft PR; выполнение на Lobachevsky/Butlerov не заявлено |
| [Identity](https://github.com/E7A-RU/identity) | Mature OIDC IdP + канонические Person/Claims/Policy, roles, organizations, recovery | Документально инициализирован 2026-10-30: README/AGENTS/maintainership/pin; **реализация и runtime НЕ подтверждены** |
| Central Audit (repo/ownership TBD) | Единый контракт события, защищённый приём, хранение, tamper evidence, fault policy | F1 priority, runtime не подтверждён; не строить общую продуктовую БД |
| [Platform](https://github.com/E7A-RU/platform) | Delivery/СГДС + trusted Jobs + Connectors + Results и реальный сквозной сценарий | Документально инициализирован 2026-10-30; **Delivery/Jobs/Connectors/Results НЕ работают по этому факту**, provider readiness отдельно |
| Billing Foundation (repo TBD) | Plans, prices, orders, subscriptions, entitlement events | F1 contracts; реальный checkout и деньги только после compliance |
| [Laboratory](https://github.com/E7A-RU/laboratory) | Product Factory: Wizard/Kits, QA, isolated Sandbox, release evidence, developer portal | Draft Product Factory, runtime не подтверждён |
| [Website](https://github.com/E7A-RU/website) | Реальный B2C discover/try/use/feedback + B2B request-automation | Отдельная программа UX/marketing/conversion с доказательными claims |
| [Reader](https://github.com/E7A-RU/reader) | Browser-only/local-first MVP; кандидат M1 | Не требует готовых Identity/Platform/Billing для основного чтения |
| [Bot Studio](https://github.com/E7A-RU/bot) | Dialogue/business-logic builder; generic adapters постепенно мигрируют в Platform | Существующий продукт сохраняет текущие runtime/DB до parity и отдельного PR |
| [Planner](https://github.com/E7A-RU/planner) | Самостоятельный планировщик | Дизайн + Starter в Draft; MVP scope отдельно не утверждён |
| [Gateway](https://github.com/E7A-RU/gateway) | Персональный WireGuard control plane без риска действующим каналам | Mock-only PR; QA, helper и реальные сети требуют отдельного разрешения |
| [Games](games-product-direction.md) | Зарегистрированное направление и потенциальная витрина внутри Website | Нет утверждённой игры или обязательного игрового backend; кандидат M1 по исследованию |
| **[E7A Admin Template](https://github.com/E7A-RU/admin-template)** (private) | Независимый reusable admin/dashboard frontend template для внутренних и будущих публичных потребителей; optional Core Starter integration | **REPO_BOOTSTRAPPED / DOC_ONLY**: [PR #1](https://github.com/E7A-RU/admin-template/pull/1) merged, `main@d56031adea734f3ae2da4942ae43f78f5b8be068`, Governance 2.0.0 pin; [AT0–AT4](admin-template-direction.md) ещё впереди; нет приложения, опубликованного пакета и runtime |

**Репозиторий и возможность** не обязательно совпадают один-к-одному: нельзя создавать Central Audit/Billing repo автоматически только из этой таблицы.

## Roadmap по проверяемым вехам (без фиктивных сроков)

### G0 — Governance adoption (сначала)

**Source documentation baseline completed 2026-10-30:** 11/11 initialized repository mains now have README/AGENTS/MAINTAINERSHIP/Governance v1.1.0 manifest; see [closeout ledger](consumer-stewardship-sprint-2026-10-30.md). Product runtime QA, Foundation F0/F1/F2 and owner's product scope decisions remain separate.

- [ ] Спроектировать [E7A Automation](engineering-automation.md) как отдельный приватный внутренний сервис (`E7A-RU/automation`), где `governance-sync` — первый модуль; репозиторий [создан](https://github.com/E7A-RU/automation), plan-only исходники в [Draft PR #1](https://github.com/E7A-RU/automation/pull/1); GitHub App, верификация приватных потребителей, canary и размещение на Infra — отдельные gates, а не автоматический эффект merge.

- [ ] Выпустить [реестр принятия стандарта](repository-impact-matrix.md) с точным baseline SHA, owner и статусами.
- [ ] В каждом существующем consumer PR сослаться на approved revision; исторический Standard v1 оставить read-only archive.
- [ ] Завершить отдельно проверяемые документальные PR; при diverged/head conflicts применять безопасную синхронизацию, не переписывая чужой runtime.
- [ ] Определить ответственных за принятие изменений и правила исключений (owner, expiry, compensating control).
- [ ] Согласовать release manifest стандарта и режим PR-only распространения без hosted CI.

**Exit:** нет конкурирующего нормативного источника; каждое изменение организационной нормы можно сопоставить с ID версии и affected products. Не является подтверждением внедрения runtime.

### F0 — Architecture & safety design

- [ ] TEN-001: Person / Account / SecurityActor / Personal Space / Organization / Workspace / tenant and scoped ownership.
- [ ] IAM-001: IdP selection, OIDC `sub`, MFA/passkeys/password/federation, account linking/recovery, trust and privacy.
- [ ] AUD-001: common event envelope, authentication, retention, tamper evidence and risk-based failure policy.
- [ ] PLT-001: transaction boundary, operation lifecycle, idempotency/UNKNOWN, trusted Jobs, Connector SDK, Results, provider capability matrix.
- [ ] INF-001: authorized runner/QA VM, host boundary, secrets and independent access recovery; no impact on KS54.
- [ ] BIL-001: commercial events and entitlement separation (not payments).
- [ ] GOV-001: risk-tier PR classifier, exact SHA evidence, temporary owner-only critical approval exception.
- [ ] Product manifest/Kit compatibility and version rules reviewed for Browser-only, Full-stack and Service/Worker.

**Exit:** versions/owners/risks/acceptance scenarios exist; no circular bootstrap dependency Identity↔Audit↔Platform/Billing.

### F1 — Foundation Integrated (minimal vertical slices, in parallel)

- [ ] Infra provides isolated self-hosted QA and shared-host safeguards; separate staging only if product-specific risk requires it.
- [ ] Identity can establish a subject through tested IdP flow; product can validate scoped access. No compulsory login for anonymous product paths.
- [ ] Audit accepts security events with agreed schema and durable degraded-mode behavior.
- [ ] Platform demonstrates **one** real authorized provider integration end-to-end: accepted → dispatched → provider receipt or UNKNOWN → result subscription/API; other Telegram/MAX/VK/email/Instagram capabilities are individually gated.
- [ ] Trusted Jobs perform a bounded idempotent task; no arbitrary code in privileged worker.
- [ ] Billing contracts support plan/order/subscription/entitlement lifecycle using synthetic events; no actual payments.
- [ ] Laboratory collects exact-SHA evidence and supports one safe Kit/QA flow; remains separate from Infra production apply.
- [ ] End-to-end integration and negative tests cover loss of audit, expiration/revoke, duplicate operations, tenant isolation, rollback expectations.

**Exit:** integrated and tested **specific slices**, not assertion that all providers and enterprise features are RUNNING.

### F2 — Operational readiness by capability

- [ ] Off-host backups exist for each stateful slice; tested restore and recorded RPO/RTO.
- [ ] Secret/key rotation and break-glass exercised without depending on failed Identity/Laboratory.
- [ ] Health, monitoring, alerting, incident contacts, bounded retries/queue backpressure and fallback tested.
- [ ] Release evidence: exact commit + immutable artifact digest, risks, compatibility, isolated QA test report; staging evidence when required by risk.
- [ ] Production action requires validated standing authorization for eligible R0/R1 **or** separately recorded owner approval for high-risk change, plus Infra verification; limited one-person exception tracked, no pretend independent reviewer.
- [ ] Rollback/drill and failure-mode evidence; no mandatory HA cluster merely to satisfy F2.

**Exit:** first essential capability can be safely operated or restored within declared targets.

### M1 — Product-in-Use (first B2C value)

- [ ] Compare Reader / Games / other B2C hypotheses using user job, accessibility, cost, privacy and build complexity.
- [ ] Select first product by documented owner decision; do not assume Games or Reader wins.
- [ ] Website provides honest discover → demo → useful product → feedback path.
- [ ] Real user success and appropriate repeat use measured with consent/privacy; no false product-market-fit claims.
- [ ] Safe second product release with rollback evidence and user feedback handled.

**Exit:** proof of real use and repeatable independent product release. This is **not** the same as F1 or F2.

### P2 — Product portfolio scaling

- [ ] Grow Library / Kit coverage and safe self-hosted QA only after evidence.
- [x] **E7A Admin Template — приватный product bootstrap:** `E7A-RU/admin-template` создан, README/AGENTS/Charter/Status/Governance pin приняты в [PR #1](https://github.com/E7A-RU/admin-template/pull/1); документационный факт, не MVP QA.
- [ ] **E7A Admin Template — AT0–AT4:** согласовать первого внутреннего/внешнего потребителя и MVP UI scope, затем независимо разрабатывать версионируемый admin/dashboard frontend поверх опубликованного Core UI; позднее — необязательная интеграция в Core Starter и отдельное решение по публичной лицензии. [Charter/этапы и ограничения](admin-template-direction.md). Без четвёртого Kit, общего admin backend и обязательных shared services.
- [ ] Migrate Bot generic adapters to Platform after parity/dual-run/rollback; keep dialog state owned by Bot.
- [ ] Add approved games, Planner or other products with independent charters.
- [ ] Expand multi-node/regions, providers and Billing payments only when owner, law, reliability and business evidence justify costs.

## Priorities and sequencing

**P0:** governance adoption, no production regressions, safety of existing hosts, F0 contracts. **P1:** minimal F1 slices plus Website B2C/B2B improvements. **P2:** F2 recoverability, M1 launch path and repeat release. **P3:** expansion/automation/scale proven by demand. These are *dependency priorities*, not hard sequential blocks: Website, discovery, Core library work and Foundation can proceed in parallel.

## Periodic review and exception policy

At each owner review: record progress as `APPROVED / PROPOSED / IMPLEMENTED SOURCE / TESTED / RUNNING / BLOCKED`, exact evidence link, next dependency, risk, next decision and estimated cost. Every exception has owner, expiry, affected rule, reason and compensating control. Update the roadmap by reviewed PR; do not rewrite the architecture baseline to match an expedient implementation.

Related: [product checklist](product-lifecycle-checklists.md), [standards propagation](standards-propagation.md), [existing M1 backlog](implementation-priorities.md), [Foundation boundary](final-architecture-baseline-2026-10-27.md).
