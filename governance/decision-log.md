# E7A — реестр решений и открытых ADR (2026-10-25)

**Статусы:** APPROVED-CONCEPT означает согласованную философию, не работающий сервис. PROPOSED и SOURCE/VERIFIED отмечать отдельно. Для публикации этих governance files в main необходим merge Draft PR #1.

| ID | Решение | Статус | Технический статус |
| --- | --- | --- | --- |
| 001 | Core — версионированный SDK/UI, не общий backend; Starter опционален | APPROVED-CONCEPT | Core packages/CLI есть в source |
| 002 | Laboratory — пользовательский продукт Wizard, QA, sandbox, Release Gate | APPROVED-CONCEPT | Portal не проверен как runtime |
| 003 | Три Kit: Browser-only, Full-stack, Service/Worker | APPROVED-CONCEPT | Генерация всех Kit не проверена |
| 004 | Infra — host/VM/provisioner/ops, научные hostnames, Lobachevsky первым | APPROVED-CONCEPT | Исходные E7A/KS54 сервисы работают отдельно, общего provisioner нет |
| 005 | Identity: единый канонический субъект для всех зарегистрированных людей/организаций и service principals | APPROVED-CONCEPT | IdP/repo ещё предстоит создать |
| 006 | Anonymous Website/local-first Reader возможны без логина | APPROVED-CONCEPT | Не вводить скрытых загрузок |
| 007 | Нет одной общей бизнес-БД; отдельное владение данными Identity/Platform/продуктов | APPROVED-CONCEPT | DR/DB ownership требуется проектировать |
| 008 | Platform как ограниченный backend-as-a-service: Delivery/Jobs/Connectors/Results | APPROVED-CONCEPT | RFC, runtime ещё нет |
| 009 | СГДС для внутренних и внешних каналов; Accepted/Delivered/Processed различны | APPROVED-CONCEPT | Технологию и SLO выбрать по ADR |
| 010 | Workers продукта остаются у продукта, generic Jobs возможно в Platform | APPROVED-CONCEPT | Не создавать отдельный worker repo без proof |
| 011 | Bot Studio остаётся специализированным dialog product, не universal integration hub | APPROVED-CONCEPT | Bot adapter/inbox/outbox уже в исходниках |
| 012 | Email adapter ≠ SMTP relay ≠ mailbox product | APPROVED-CONCEPT | Новая почта не развёрнута |
| 013 | Governance canon в .github, consumers pin revision, self-hosted diff checker предлагает PR | APPROVED-CONCEPT | Автоматический checker пока не реализован |
| 014 | GitHub Actions CI запрещён, Dependabot PR only, owner-approved releases | EXISTING POLICY | Ничего не включать |
| 015 | Brand Policy отдельно от code licence, UI public UNLICENSED | EXISTING POLICY | PR #1 содержит брендовые правила |
| 016 | Physical hypervisor/Kubernetes не прибавляет CPU/RAM/disk одного сервера | ARCHITECTURAL OBSERVATION | K3s лишь при multi-node/operational demand |

| 017 | Website — публичная витрина возможностей, продуктов, доказательных кейсов и входов для B2B/B2C | PROPOSED-MARKET-POSITIONING | Production Website существует; новая маркетинговая роль/конверсия и CTA ещё не подтверждены |
| 018 | Ценность E7A для целевых заказчиков: быстрая, надёжная, качественная автоматизация под ключ без ненужной сложности и переплат | OWNER-DIRECTION / CLAIMS-REQUIRE-EVIDENCE | Цены, сроки, гарантии, сегменты и маркетинговые заявления должны иметь доказательства |
| 019 | Laboratory — вход для разработчиков/партнёров (B2D) и внутреннего штата (B2E), pipeline пополнения команды и выпуска клиентских решений | PROPOSED-PRODUCT-EXTENSION | Wizard/QA charter утверждён концептуально; recruitment/client workspace и роли не реализованы |
| 020 | Публичный Demo Catalog и раздельные demo/trial: synthetic showcase, guided B2B demo, invite-only developer sandbox | PROPOSED | Портал/демо поддомен не развёрнуты; сценарии и ресурсные лимиты требуют решения |
| 021 | Website/Identity/Laboratory не смешивают маркетинговый lead, кандидатскую запись, tenant-клиента и developer privileges | PROPOSED-SECURITY-GUARDRAIL | Ограничение доступа по умолчанию, privacy review и identity membership обязательны до подключения |

## Открытые ADR

Website: приоритет B2B/B2C/B2D, маркетинговые сегменты, реальные кейсы, лидогенерация/CRM и privacy; Laboratory: open signup vs invite-only, кандидаты/HR data owner, staff roles, клиентский кабинет; Demo: публичная точка входа и первый безопасный сценарий, TTL/budget/anti-abuse, synthetic vs live integration. Эти решения **ещё не подтверждены** как реализованная маркетинговая стратегия или запущенный интерфейс.


Identity IdP/assurance/privacy, service auth, migration Bot login; Platform delivery durability/UNKNOWN/provider ACK/idempotency, secure connector SDK; Jobs execution/leases; почта provider vs MTA; Product Kit code/profile migrations; Laboratory QA/runner sandbox; Infra DNS/TLS ownership, secrets, backup RPO/RTO, K3s threshold; schema и immutable versioning standards.

## Существующие Draft на момент аудита

[Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Org #1](https://github.com/E7A-RU/.github/pull/1), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1). Состояние PR может измениться позднее; проверять живое состояние перед merge. Прежний Core Platform RFC не удалять без явной reconciliation; исходные README и Brand Policy не переписывать.
