# E7A Roadmap — от Governance к работающим продуктам

**Статус:** OWNER-APPROVED STRATEGY / PROPOSED DELIVERY PLAN (2026-10-30). Канонический baseline: [Governance at immutable SHA](https://github.com/E7A-RU/.github/tree/b39ed4aabf97e45edaa0762a29ab57c69f8bbafd/governance). Ниже перечислены **целевые критерии**, а не выполненные этапы, бюджет, обещания сроков или полномочия на production.

**Практические шаблоны:** [Product Charter](templates/product-charter.md) → [checklists](product-lifecycle-checklists.md) → [Release Gate](templates/release-gate.md).

## Главные цели и инварианты

- **B2C-first**, B2B автоматизация — самостоятельное второе коммерческое направление; Website развивается параллельно Foundation.
- **Независимые продукты:** собственный домен, данные, авторизация внутри продукта, версии и релизы. Общие сервисы подключаются по необходимости; анонимные Website/Reader/игры не требуют постоянного общего backend.
- **Foundation** — инженерная программа, а **не** обязательный единый репозиторий, монолит, уже выпущенная версия ПО или новый продукт.
- **Безопасность и правдивый статус:** APPROVED → IMPLEMENTED SOURCE → TESTED → STAGING VERIFIED → RUNNING VERIFIED являются разными доказательствами. GitHub Actions hosted CI запрещён, отдельная разрешённая ручная публикация пакетов Core — исключение. Любой production apply требует отдельного owner approval.
- **Любая техническая веха проходит через [Product Lifecycle & Checklists](product-lifecycle-checklists.md).** Не смешивать governance adoption PR с релизом продукта.

## Портфель и этапы

| Контур / репозиторий | Целевой результат | Сегодняшний доказательный статус / последовательность |
| --- | --- | --- |
| [Governance](README.md) / .github | Утверждённая версия правил и controlled change management | Первичный baseline принят; consumer adoption и синхронизация ещё проходят PR |
| [Core](https://github.com/E7A-RU/core) | Версионированные библиотеки и опциональный Product Starter | Репозиторий существует, источник UI и server contracts; каждый новый выпуск отдельно проверяется |
| [Infra](https://github.com/E7A-RU/infra) | Безопасные ресурсы, QA VM, секреты, backup/restore, staging, controlled apply, node protocol | Целевая концепция / Draft PR; выполнение на Lobachevsky/Butlerov не заявлено |
| [Identity](https://github.com/E7A-RU/identity) | Mature OIDC IdP + канонические Person/Claims/Policy, roles, organizations, recovery | Репозиторий есть, но на дату ревизии пуст; bootstrap ещё требуется |
| Central Audit (repo/ownership TBD) | Единый контракт события, защищённый приём, хранение, tamper evidence, fault policy | F1 priority, runtime не подтверждён; не строить общую продуктовую БД |
| [Platform](https://github.com/E7A-RU/platform) | Delivery/СГДС + trusted Jobs + Connectors + Results и реальный сквозной сценарий | Репозиторий есть, но на дату ревизии пуст; provider readiness подтверждать по отдельности |
| Billing Foundation (repo TBD) | Plans, prices, orders, subscriptions, entitlement events | F1 contracts; реальный checkout и деньги только после compliance |
| [Laboratory](https://github.com/E7A-RU/laboratory) | Product Factory: Wizard/Kits, QA, isolated Sandbox, release evidence, developer portal | Draft Product Factory, runtime не подтверждён |
| [Website](https://github.com/E7A-RU/website) | Реальный B2C discover/try/use/feedback + B2B request-automation | Отдельная программа UX/marketing/conversion с доказательными claims |
| [Reader](https://github.com/E7A-RU/reader) | Browser-only/local-first MVP; кандидат M1 | Не требует готовых Identity/Platform/Billing для основного чтения |
| [Bot Studio](https://github.com/E7A-RU/bot) | Dialogue/business-logic builder; generic adapters постепенно мигрируют в Platform | Существующий продукт сохраняет текущие runtime/DB до parity и отдельного PR |
| [Planner](https://github.com/E7A-RU/planner) | Самостоятельный планировщик | Дизайн + Starter в Draft; MVP scope отдельно не утверждён |
| [Gateway](https://github.com/E7A-RU/gateway) | Персональный WireGuard control plane без риска действующим каналам | Mock-only PR; QA, helper и реальные сети требуют отдельного разрешения |
| [Games](games-product-direction.md) | Зарегистрированное направление и потенциальная витрина внутри Website | Нет утверждённой игры или обязательного игрового backend; кандидат M1 по исследованию |

**Репозиторий и возможность** не обязательно совпадают один-к-одному: нельзя создавать Central Audit/Billing repo автоматически только из этой таблицы.

## Roadmap по проверяемым вехам (без фиктивных сроков)

### G0 — Governance adoption (сначала)

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

- [ ] Infra provides an authorized isolated test/staging environment and safeguards for shared hosts.
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
- [ ] Staging release evidence: exact commit + immutable artifact digest, risks, compatibility, test report.
- [ ] Production action requires separately recorded owner approval and Infra verification; limited one-person exception tracked, no pretend independent reviewer.
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
- [ ] Migrate Bot generic adapters to Platform after parity/dual-run/rollback; keep dialog state owned by Bot.
- [ ] Add approved games, Planner or other products with independent charters.
- [ ] Expand multi-node/regions, providers and Billing payments only when owner, law, reliability and business evidence justify costs.

## Priorities and sequencing

**P0:** governance adoption, no production regressions, safety of existing hosts, F0 contracts. **P1:** minimal F1 slices plus Website B2C/B2B improvements. **P2:** F2 recoverability, M1 launch path and repeat release. **P3:** expansion/automation/scale proven by demand. These are *dependency priorities*, not hard sequential blocks: Website, discovery, Core library work and Foundation can proceed in parallel.

## Periodic review and exception policy

At each owner review: record progress as `APPROVED / PROPOSED / IMPLEMENTED SOURCE / TESTED / RUNNING / BLOCKED`, exact evidence link, next dependency, risk, next decision and estimated cost. Every exception has owner, expiry, affected rule, reason and compensating control. Update the roadmap by reviewed PR; do not rewrite the architecture baseline to match an expedient implementation.

Related: [product checklist](product-lifecycle-checklists.md), [standards propagation](standards-propagation.md), [existing M1 backlog](implementation-priorities.md), [Foundation boundary](final-architecture-baseline-2026-10-27.md).
