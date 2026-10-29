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

| 017 | Website — публичная витрина для B2C в приоритете и B2B как второго направления | APPROVED-PRIORITY / PROPOSED-TACTICS | Website production существует; новый маркетинговый funnel и CTA ещё не внедрены |
| 018 | Ценность E7A: полезные B2C-продукты и качественная автоматизация под ключ без ненужной сложности/переплат; B2C-first | APPROVED-DIRECTION / CLAIMS-REQUIRE-EVIDENCE | Цены, сроки, гарантии и конкретный сегмент требуют подтверждений |
| 019 | Laboratory — вход для разработчиков/партнёров (B2D) и внутреннего штата (B2E), pipeline пополнения команды и выпуска клиентских решений | PROPOSED-PRODUCT-EXTENSION | Wizard/QA charter утверждён концептуально; recruitment/client workspace и роли не реализованы |
| 020 | Публичный Demo Catalog и раздельные demo/trial: synthetic showcase, guided B2B demo, invite-only developer sandbox | PROPOSED | Портал/демо поддомен не развёрнуты; сценарии и ресурсные лимиты требуют решения |
| 021 | Website/Identity/Laboratory не смешивают маркетинговый lead, кандидатскую запись, tenant-клиента и developer privileges | PROPOSED-SECURITY-GUARDRAIL | Ограничение доступа по умолчанию, privacy review и identity membership обязательны до подключения |

| 022 | Первая цель M1 Product-in-Use: один B2C продукт, Website → Demo → Use → Feedback → безопасный повторяемый выпуск | APPROVED-GOAL / FLAGSHIP-PROPOSED | Reader — кандидат, флагман ещё не выбран; реальные KPI требуют baseline |
| 023 | Базовая структура команды для M1 — функциональные роли с предварительной оценкой около 4.0 FTE, возможно совмещение | PROPOSED-CAPACITY | Это не штат/бюджет/зарплаты; нужна проверка ресурсов и назначение owners |
| 024 | Собственный Brandbook на базе Core/Website, референсы носителей и обязательные концепции худи | APPROVED-DIRECTION / DESIGNS-PROPOSED | Макеты, цвета ткани, тираж, rights, samples и SKU не утверждены |

| 025 | Разработать самостоятельный оригинальный E7A Stickerpack (12 сюжетов MVP) как проект брендбука | APPROVED-DIRECTION / ART-NOT-CREATED | Нарисованные и опубликованные стикеры, права/QA/форматы ещё требуют работы |
| 026 | Создать юридический контур РФ: НПД/ИП/ООО, товарный знак, авторское право, возможный промышленный образец, data/product compliance | APPROVED-RESEARCH / IMPLEMENTATION-OPEN | Правовое исследование в legal/, но нет регистрации бизнеса, экспертизы по конфликтам или поданной заявки |
| 027 | Для защиты E7A приоритетно проверить словесный и изобразительный товарные знаки; «патента на бренд в целом» нет | LEGAL-ASSESSMENT / CLEARANCE-OPEN | Физлицо вправе подать заявку; нужны поиск, права и классы МКТУ |
| 028 | Продажа фирменного худи: отдельная оценка Честного знака, соответствия, маркировки, формы бизнеса и налогов | COMPLIANCE-GATE / SALES-NOT-APPROVED | Никаких утверждённых SKU/тиражей/заказов |

| 029 | Практический trademark-файлинг физлица E7A отдельно от регистрации НПД/ИП/ООО; работа по найму совместима с самостоятельным доходом в рамках НПД | VERIFIED-LEGAL-ROUTE / INDIVIDUAL-FACTS-OPEN | Никаких поданных заявок или регистрации НПД; нужны clearance, классы, actual owner и employment IP review |

| 030 | Предварительный clearance E7A: точных совпадений по отчёту не выявлено; риск EA7 в классе 25 и различительной способности E7A | RESEARCH-RECORDED / CLEARANCE-INCOMPLETE | Поданы 0 заявок; WIPO/ФИПС карточки, тысячи fuzzy/phonetic/image выдач и статусы требуют повторной проверки поверенным. Классы 9/42 — первый предметный фокус |

| 031 | Первая регистрация E7A — **один комбинированный знак E7AMark + E7A в МКТУ 42**, после clearance и фиксации макета | APPROVED-STRATEGY / NOT-FILED | Одна заявка: 17 000 ₽ первоначально и ещё 18 000 ₽ при успехе; не равна двум самостоятельным знакам. [План](../legal/e7a-combined-mark-first-filing-strategy.md) |

## Решения владельца 2026-10-27 (раунды 1–10)

**APPROVED-DIRECTION, NOT IMPLEMENTED:** [единая консолидированная запись с границами, статусами и миграцией](owner-decisions-2026-10-27.md). Исторические решения и гипотезы выше сохраняются. Для ранее записанных PROPOSED положений, которым новые решения противоречат, применяются пометки **SUPERSEDED-AS-PROPOSAL** только в части названных границ, не как переписывание исторического контекста.

- 032 — Architecture-first, F1/F2/M1, B2C-first и Website dual-route.
- 033 — Core = versioned shared libraries/contracts/optional Starter, не runtime.
- 034 — Identity Person vs Account vs Actor; расширяемый профиль, проверенные привязки, MFA/passkeys/password/OIDC/federation, multi-organisation scopes.
- 035 — Central Audit capability и единый защищённый контракт событий с ранней реализации.
- 036 — Platform four contours upfront; transactional internal state and external UNKNOWN, SDK + provider adapters, multi-step workflows, independently aggregated results.
- 037 — Bot Studio целевым образом = конструктор бизнес-логик; общие коннекторы переносятся в Platform отдельными проверяемыми PR, исходный runtime неизменён.
- 038 — Laboratory/Infra: приглашения, QA VM на Lobachevsky, Dev/Staging/Production, multi-node API поэтапно.
- 039 — R0/R1 возможен auto-merge после локальных/self-hosted exact-head gates; production всегда требует owner approval; 4 FTE ориентир, фактически один человек.
- 040 — Billing Foundation включён в F1 как контракты и минимально достаточная модель, платежи только после compliance gate.
- 041 — Физлицо как первоначальный планируемый правообладатель, proprietary default, unified contributor framework плюс договоры по проектам; фактическую цепочку прав и юридические условия проверить отдельно.

## Заключительные решения владельца — 2026-10-27

**APPROVED-DIRECTION / GOVERNANCE DRAFT:** [final architecture baseline](final-architecture-baseline-2026-10-27.md). Решения 042–049:

| ID | Decision | Consequence |
| --- | --- | --- |
| 042 | Person → Personal Space / Organisation → Workspaces → product resources | Ownership context agreed; canonical tenant mapping requires TEN-001 ADR |
| 043 | Mature OIDC IdP + E7A Person/Claims/Policy | Own authentication cryptography prohibited; IdP vendor not yet selected |
| 044 | Trusted Platform Jobs only; arbitrary user code in isolated Sandbox | Host privileges and cross-tenant secrets never exposed to untrusted jobs |
| 045 | Initial Billing: plans/orders/subscriptions/entitlements/events, payment after compliance | No payment acceptance or ledger completion claimed |
| 046 | Central Audit failure behavior risk-dependent | Critical actions fail-closed or bounded separately audited emergency procedure |
| 047 | F2 tested recovery with RPO/RTO, off-host backups, no mandatory HA | Downtime allowed per explicit SLO and disaster drill |
| 048 | Temporary owner-only critical release approval, with compensating evidence and reassessment | Exception to prior independent two-human requirement; not misrepresented as independent review |
| 049 | Multi-region architecture, first market unselected | Legal residency, customer region and payment jurisdiction must be chosen before launch |

## Новое продуктовое направление Games — 2026-10-27

| ID | Decision | Consequence |
| --- | --- | --- |
| 050 | Games добавлено как самостоятельное направление / портфель потенциальных независимых игровых продуктов | OWNER-REQUESTED DIRECTION; жанры, аудитория, репозитории, монетизация, статус M1 — **OPEN**. [Games charter](games-product-direction.md); никаких созданных runtime/repo. |

## Gateway — отдельный существующий продукт (2026-10-30)

| ID | Решение | Статус |
| --- | --- | --- |
| 054 | E7A Gateway добавлен в продуктовую линейку как самостоятельный WireGuard control-plane продукт для семейных устройств на Butlerov; **не** общий API Gateway и не Infra | EXISTING REPO / DESIGN IN DRAFT PR / IMPLEMENTATION UNVERIFIED. [Границы и риски](gateway-product-direction.md). Запуск, миграция peers, изменение сетей и merge не разрешены. |

## Открытые ADR

Website: **B2C приоритет согласован**, открыты выбор флагмана, каналов, маркетинговые сегменты, реальные кейсы, лидогенерация/CRM и privacy; Laboratory: open signup vs invite-only, кандидаты/HR data owner, staff roles, клиентский кабинет; Demo: публичная точка входа и первый безопасный сценарий, TTL/budget/anti-abuse, synthetic vs live integration. Эти решения **ещё не подтверждены** как реализованная маркетинговая стратегия или запущенный интерфейс.


Identity IdP/assurance/privacy, service auth, migration Bot login; Platform delivery durability/UNKNOWN/provider ACK/idempotency, secure connector SDK; Jobs execution/leases; почта provider vs MTA; Product Kit code/profile migrations; Laboratory QA/runner sandbox; Infra DNS/TLS ownership, secrets, backup RPO/RTO, K3s threshold; schema и immutable versioning standards.

Бренд и право: [Stickerpack](../brandbook/stickerpack.md), [Legal overview](../legal/README.md), [товарный знак/патенты](../legal/brand-ip-and-patents-russia.md), [налоговый режим](../legal/business-structure-russia.md), [compliance](../legal/product-compliance-russia.md). Конкретный юридический статус и принадлежность IP остаются фактическими вопросами, не вытекают из этого ADR.

## Существующие Draft на момент аудита

[Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Org #1](https://github.com/E7A-RU/.github/pull/1), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1). Состояние PR может измениться позднее; проверять живое состояние перед merge. Прежний Core Platform RFC не удалять без явной reconciliation; исходные README и Brand Policy не переписывать.


Стратегия: [M1 и этапы](growth-strategy-and-first-milestone.md); [roles/FTE](team-and-capacity.md); бренд: [Brandbook](../brandbook/README.md), [Hoodie](../brandbook/merchandise-and-hoodie.md).

## Решения владельца по назначению и управлению Governance (2026-10-28)

| ID | Решение | Статус |
| --- | --- | --- |
| 051 | PR #1 предназначен для утверждения первичных организационных и архитектурных стандартов E7A, не runtime readiness | OWNER-APPROVED / MERGE-PENDING |
| 052 | Жёстко закрепляются архитектурные и security-инварианты, техническая реализация остаётся изменяемой через ADR | OWNER-APPROVED / MERGE-PENDING |
| 053 | Founder / Owner E7A лично отвечает за дальнейшее развитие и утверждение обязательных стандартов | OWNER-APPROVED / MERGE-PENDING |

