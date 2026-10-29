# E7A Product Kits — эталонные дорожные карты внедрения

**Статус: PROPOSED EXECUTION BASELINES for already approved three Kit categories.** Product Factory выбирает Kit; Core предоставляет **опциональный** исходный Starter; каждый продукт владеет исходниками и релизами; Infra управляет инфраструктурой; Laboratory собирает QA evidence. Kit означает **стартовую конфигурацию и acceptance gates**, а не готовое приложение/сервис или mandatory backend.

**Общие правила:** [Governance](README.md), [Product lifecycle](product-lifecycle-checklists.md), [Backend Reference](backend-reference-architecture.md), [Product Charter](templates/product-charter.md), [Release Gate](templates/release-gate.md). Метки `PASS` только с тестовым evidence exact SHA. GitHub-hosted Actions CI запрещён; production всегда требует отдельного решения Owner.

## Общий путь для любого Kit

```text
Discovery & User Job
  → Product Charter + owner scope approval
  → choose Kit + pin Governance full SHA + exact Core versions
  → architecture/data/privacy ADR + release/environment plan
  → implementation in independent product repository
  → local/self-hosted exact-SHA checks + negative tests
  → isolated QA and staging acceptance when applicable
  → separate owner production approval → Infra controlled apply
  → verify real user value/health → second safe release
```

**Варианты без пользователя в рантайме:** Browser-only может выпускаться как immutable статический сайт или клиентское приложение без API; Service/Worker может не иметь UI; Full-stack предполагает независимые API+данные. Модель доступа согласуется с функцией, не навязывается искусственно.

## 1. Browser-only Kit

**Для чего:** локальный Reader, offline/PWA, browser-first игры без аккаунта, статические/интерактивные автономные приложения. Не требует PostgreSQL, API, общего аккаунта, Platform или фонового backend worker.

### Эталонный стек

- TypeScript strict + React или другой мотивированный UI; Core UI exact-version и дизайн-токены, когда они нужны.
- Static host или Next.js static-compatible path; **если SSR/Node runtime нужен, это уже не полностью browser-only** и соответствующие runtime gates обязательны.
- Storage `memory / localStorage / IndexedDB` только по privacy policy, с квотами, схемами/миграциями и export/delete при необходимости.
- Client-side parsing/crypto выполняется в безопасном контексте с ограничением размеров, CPU, файлов, ZIP/HTML и сетевых запросов.
- PWA/offline — опциональная capability; не обещать без offline and cache-update evidence.

### Эталонный roadmap

**B0 — Discovery & Charter**
- [ ] Записать локальный пользовательский сценарий и способ проверить пользу без сервера.
- [ ] Подтвердить отсутствие необходимости серверной синхронизации, покупок, Identity и собственной БД.
- [ ] Выбрать browsers/devices, privacy, accessibility targets, языки, форматы, legal/IP.

**B1 — Core Starter & Design**
- [ ] Создать отдельный repo/branch из Browser-only Kit или минимального шаблона Core.
- [ ] Pin Core UI и Governance SHA + frozen lockfile; определить local state и устойчивость к смене версии.
- [ ] Настроить CSP, sandbox/untrusted file treatment, безопасную работу с Blob/ObjectURL и error boundaries.

**B2 — Vertical slice**
- [ ] Реализовать один реальный happy path полностью в браузере; никаких неожиданных uploads.
- [ ] Добавить app state, settings, controls, пустые/некорректные данные, retry/failure UX.
- [ ] Если используются IndexedDB или SW — версия схемы, cache invalidation и приватный export/delete.

**B3 — QA**
- [ ] Typecheck/lint/unit/build, pinned deps security audit, browser E2E на desktop/mobile, accessibility.
- [ ] Negative tests: вредоносные/крупные файлы, parser budget, XSS, авария quota/offline, обновления SW.
- [ ] Network inspection: проверка отсутствия передачи личных файлов/данных без явного согласия.

**B4 — Distribution/Release**
- [ ] Staging/static preview immutable SHA, robots/canonical/privacy notices, cache headers, 404/health.
- [ ] Проверить поведение без обязательных Foundation сервисов и без логина.
- [ ] Production owner approval; отдельно URL/DNS/TLS change через Infra при необходимости.

**B5 — Product-in-Use**
- [ ] Реальные пользователи выполняют сценарий; privacy-respecting feedback, наблюдаемая полезность.
- [ ] Второй безопасный релиз и откат static artifacts/cache; повторная browser regressions.
- [ ] Документировать основание будущей миграции в Full-stack, если появится sync/account.

**Exit:** полезное автономное приложение с доказанным браузерным поведением, privacy и повторяемым релизом. **Не требует Postgres.**

## 2. Full-stack Kit

**Для чего:** Planner после owner-approved scope, бизнес-системы/кабинеты, персональные профили, продукты с учётными записями и серверными транзакциями. Gateway имеет Full-stack control plane, но требует **дополнительных privileged/network gates**, потому не является универсальным продуктовым Starter.

### Эталонный стек

- Frontend: TypeScript + React/Next.js с Core UI по exact-version, accessibility и безопасными client/server boundaries.
- Backend: отдельный **TypeScript Node API** (Fastify с типизированным schema validation по умолчанию; NestJS по ADR), модули `transport → application → domain → adapters`. Не смешивать опасные системные операции с web handlers.
- DB: **dedicated PostgreSQL database/role per product**, `pg` + SQL-first checksum migrations, constraints, explicit transactions, transactional outbox, no production auto-sync. СУБД может размещаться в общем кластере Infra при гарантированной изоляции.
- Identity: при реальном логине зрелый OIDC IdP + E7A Person/Claims/Policy, разные Person/Account/SecurityActor/tenant; domain ACL в продукте.
- Background: продуктовый worker для своего домена, Platform trusted Jobs/Connectors по реальной необходимости.
- Deploy: immutable containers/artifacts, DB migration gate, application health/readiness, secrets and reverse proxy Infra.

### Эталонный roadmap

**F0 — Product scope**
- [ ] Product Charter, пользователи/роли/tenant, data flows и бюджет серверных ресурсов.
- [ ] Определить offline/anonymous flows и обязательность Identity только для авторизованных действий.
- [ ] Утвердить MVP и data ownership; отдельные compliance решения для персональных/финансовых данных.

**F1 — Contracts & DB**
- [ ] Схема API (OpenAPI + compatible errors), продуктовая domain model и независимая PostgreSQL DB.
- [ ] Выбрать поддерживаемую PostgreSQL major, `pg` и SQL миграции с ledger/checksum, DDL indexes/unique/FK/constraints.
- [ ] Развести DB runtime/migration/backup roles; запретить cross-product credentials.
- [ ] Согласовать privacy retention/erase, tenant scope/row policies, backup RPO/RTO.

**F2 — Secure skeleton**
- [ ] Создать UI/API и auth/session boundary; role+resource-scoped authorization на сервере.
- [ ] Health/readiness, structured logs, correlation ID, error model, secrets injection; no secret in repo.
- [ ] DB transaction helper, domain repository, migration CLI, deterministic test fixtures.

**F3 — First useful vertical slice**
- [ ] Один user journey: frontend → authenticated API → PostgreSQL transaction → UI confirmation.
- [ ] Record local audit + outbox in transaction; idempotency/expected revision where actions may repeat.
- [ ] If using Platform, use bounded provider adapter; failure/UNKNOWN and retry reconciliation explicitly tested.
- [ ] Never move payment/identity/domain financial authority into product by accident.

**F4 — Negative & integration QA**
- [ ] Frozen install + types/lint/build/unit/e2e, isolated Postgres and migrations from scratch, drift/checksum and replay.
- [ ] Test unauthorized cross-tenant reads/writes, CSRF, session expiry/revoke, duplicate requests, row-lock/concurrency, malformed inputs, provider failure.
- [ ] Restore test from off-host encrypted backup; test schema compatibility and rollback under realistic data.

**F5 — Staging and release**
- [ ] Separate Development/Staging/Production, QA VM isolated from production secrets.
- [ ] Infra prepares vhost/TLS, product DB, secret refs, resource budget, migration plan, immutable image SHA/digest.
- [ ] Staging end-to-end and restore proof → owner production approval → controlled release with rollback.
- [ ] Confirm privacy notices, data deletion/support, monitoring, error budget and alerts.

**F6 — Repeatable operation**
- [ ] First real user completes end-to-end journey; product analytics/minimal data.
- [ ] Second independent release validates schema expand/contract, backup restore, operational support.
- [ ] Only then consider scaling workers, queue, caching, read replicas or extra nodes.

**Exit:** operating stateful product; PostgreSQL data isolation, restore, security and reliable release **tested**, not just documented.

## 3. Service/Worker Kit

**Для чего:** независимый API, Bot runtime components, bounded asynchronous processor, provider integration, webhook ingestion, schedule/job or event service. A Service/Worker may have no public UI. Stateful services **PostgreSQL-first**, but pure compute can have no DB after ADR.

### Эталонный стек

- TypeScript/Node with typed service contracts, service identities and headless health/metrics.
- API mode: HTTP/OpenAPI with authN/authZ, rate/egress and idempotency; webhook mode: HMAC/signed provider proof, replay protection, inbox.
- Durable mode: product-owned PostgreSQL **outbox + inbox + leases/heartbeat + idempotency receipts**; dedicated message broker when throughput/failure semantics justify it. Existing Bot uses RabbitMQ in its own boundary; not automatically a Platform-owned broker.
- Jobs: trusted pre-registered executable handlers with explicit permissions; **untrusted arbitrary code only in isolated sandbox** (not privileged worker).
- Connector SDK/API/provider versions separated; platform capability check and credentials owned per instance/product/org, never E7A global secrets.

### Эталонный roadmap

**S0 — Service Charter**
- [ ] Define trigger (HTTP/webhook/schedule/event), input/output, service owner, exact SLA/backpressure/failure expectations.
- [ ] Show why this is its own service rather than function in existing product; data/tenant/security boundaries.
- [ ] Determine durable vs stateless: if stateless, write down the limits of lost work; if durable, use PostgreSQL-first.

**S1 — Versioned contracts**
- [ ] OpenAPI/JSON Schema/event envelopes, tenant/product/actor/capability IDs and correlation.
- [ ] Auth: scoped service identity, signed webhooks, retries/TTL and replay prevention.
- [ ] Define operation statuses `PENDING / ACCEPTED / DISPATCHED / SUCCEEDED / FAILED / UNKNOWN`, per-item Result and aggregate Result; no blanket exactly-once.

**S2 — State machine & worker**
- [ ] Implement worker with bounded concurrency, timeouts, cancel/retry/DLQ, leases, uniqueness and retention.
- [ ] If DB needed: SQL migrations, PK/UNIQUE/FK, outbox/inbox and transaction; idempotent operator effects.
- [ ] Provider adapters parse external results honestly, including missing receipts, partial success and UNKNOWN/reconciliation.
- [ ] Narrow egress/network permissions and secret broker; privileged host APIs only through reviewed helper.

**S3 — Contract/chaos QA**
- [ ] Typecheck/lint/build/unit, schema/consumer contracts and dependency audit.
- [ ] Kill worker before/after DB commit, before/after ack, during provider timeout, duplicate webhook, callback delay, rate-limit/429, DLQ replay.
- [ ] Test tenant isolation, token/key rotation, malicious payloads, abuse/rate limits, runaway retry and operational budgets.
- [ ] Test Postgres restore/backup if stateful and broker failure/restart if used.

**S4 — Staging/service rollout**
- [ ] Exact image digest, secret refs and minimal runtime/service identity; no public exposure unless required.
- [ ] Deploy to isolated staging with fake/sandbox provider where available; verify metrics, alerting, failed job visibility.
- [ ] Feature gate/canary, owner production approval via Infra, graceful drain/rollback and reconciliation after deploy.

**S5 — Operational scale**
- [ ] Observed throughput and failure percentages; adjust concurrency/DB pool/broker deliberately.
- [ ] Support versioned consumer upgrade, deprecation and data retention; second independently tested release.
- [ ] Migrate common Bot adapters into Platform only with parity and staged rollback evidence; leave domain dialog engine in Bot.

**Exit:** isolated headless service with explicit operation results, replay-safe effects, failure/restore evidence and safely managed lifecycle.

## Kit selection decision table

| Criteria | Browser-only | Full-stack | Service/Worker |
| --- | --- | --- | --- |
| Public end-user UI | Usually yes | Usually yes | Optional/no |
| Own API/server | Not for baseline | Yes | Yes or headless runtime |
| PostgreSQL | No mandatory DB | **Recommended default** | **Default if durable state** |
| Identity | Optional/anonymous | If named users/actions | Scoped service principals where required |
| Infrastructural QA | Browser/privacy/E2E | API/DB/tenant/restore | Idempotency/chaos/provider/lease |
| Risk gate | Client/data/privacy | Data/auth/ops | Egress/side effects/retries |
| Example | Reader, simple Games | Planner, Gateway control plane | Bot workers, Platform connectors |

## Kit compatibility and non-goals

- Browser-only is NOT a lower quality tier. A high-quality client app can be fully offline.
- Full-stack does NOT mean one shared database, identity system, messaging queue or proxy for all products.
- Service/Worker does NOT authorize a privileged runner to execute arbitrary external/user code.
- Laboratory **orchestrates** Starter + QA + evidence and presents Kit choices; Core versions the underlying templates; Infra owns runtime and resources.
- Cross-Kit upgrade is **new architecture ADR**, not hidden scope creep. E.g. Reader may add opt-in sync as separately authorized Full-stack capability while preserving local-only reading.
- Exact tool and framework versions are set per product release/ADR. This roadmap does not assert current implementations use the stack or have passed the gates.

See [Backend Reference](backend-reference-architecture.md), [Product checklist](product-lifecycle-checklists.md), [Roadmap](roadmap.md), [Growth/M1](growth-strategy-and-first-milestone.md).
