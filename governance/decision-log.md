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

## Открытые ADR

Identity IdP/assurance/privacy, service auth, migration Bot login; Platform delivery durability/UNKNOWN/provider ACK/idempotency, secure connector SDK; Jobs execution/leases; почта provider vs MTA; Product Kit code/profile migrations; Laboratory QA/runner sandbox; Infra DNS/TLS ownership, secrets, backup RPO/RTO, K3s threshold; schema и immutable versioning standards.

## Существующие Draft на момент аудита

[Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Org #1](https://github.com/E7A-RU/.github/pull/1), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1). Состояние PR может измениться позднее; проверять живое состояние перед merge. Прежний Core Platform RFC не удалять без явной reconciliation; исходные README и Brand Policy не переписывать.
