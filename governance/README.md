# E7A Governance — единый источник архитектурных решений

**Status: owner-approved Governance v2.1.0**, effective 2026-11-06. Immutable normative SHA: `c26fc1daea258a8ba63b61ea069664f6d9a51da2` ([release descriptor](standards-release.json), [migration notes](releases/2.1.0.md), Owner Decision 063). This backwards-compatible MINOR release adds E7A-wide dependency ownership: Core publishes independent SDKs and optional Starter defaults; each existing product owns its framework/runtime pins, security QA and release. It preserves approved Governance 2.0.0 risk-gated deployment policy; no automatic product updates, new write privileges, hosted Actions CI or production activation.

## Иерархия источников истины

1. **Этот репозиторий, governance/** — организация, инженерная философия, общие границы, контракты интеграции между системами, политика безопасности и порядок решений.
2. **E7A-RU/core** — исполняемые исходники версионированных SDK, UI, типизированных server contracts и текущего опционального Product Starter.
3. **E7A-RU/laboratory** — реализация Wizard, QA, release evidence, approvals, реестр продуктовых потребителей стандартов.
4. **E7A-RU/infra** — реализация декларативной инфраструктуры, серверов, runtime, мониторинга, резервного копирования и deployment.
5. **Identity / Platform (репозитории созданы, пока не имеют подтверждённого runtime)** — самостоятельные контракты и собственные данные после отдельных ADR. Central Audit и Billing Foundation также самостоятельные обязанности, их реализация не подтверждена.
6. **Каждый продукт** — собственные схемы данных, процессы, бизнес-логика, API enforcement и релизные материалы; по Decision 064 централизованные экосистемные grants и разрешения на ресурсы принадлежат Identity после Governance 3.0.0 adoption.

Общий стандарт определяет **что и зачем**, продуктовый репозиторий — **как реализовано у него**. Локальные документы дополняют, но не ослабляют организационные политики. При несовместимости обязательна запись о конфликте и отдельный ADR; не менять десятки README без контроля версии.

## Предлагаемое следующее нормативное изменение

**Решение владельца 10.11.2026:** [GOV-AUTHZ-001 — централизованная авторизация](adr/GOV-AUTHZ-001-centralized-authorization.md) и [матрица влияния на все контуры](central-authorization-impact-2026-11-10.md). Это owner-approved *direction*, **не** новая опубликованная версия. До отдельного immutable release действует Governance v2.1.0. Product/local ACL мигрируют только адресно и после QA; производственные релизные ограничения не ослаблены.

## Документы

- **[E7A Admin Template — концепция и будущие этапы](admin-template-direction.md)** — owner-requested план необязательного шаблона административных интерфейсов для внутреннего/публичного использования; пока не реализован и не публиковался.

- **[Упрощённая политика release/deploy для всей линейки](release-deployment-policy.md)** — owner-directed изменение: self-hosted QA → build → production без обязательного staging и второго клика при разрешённом R0/R1; **опубликована как нормативное направление Governance v2.0.0**, но не активирует release-процессы без отдельного включения для каждого продукта.

- **[Product Stewardship: единые правила ВП/агента и владельцев продуктов](product-stewardship.md)**
- **[Спринт внедрения и QA/PR реестр](consumer-stewardship-sprint-2026-10-30.md)**
- [Шаблон локального сопровождения каждого продукта](templates/product-maintainership.md)

- **[Эталонная backend-архитектура E7A: TypeScript + PostgreSQL-first](backend-reference-architecture.md)**
- **[Три Product Kit: roadmap Browser-only / Full-stack / Service-Worker](kit-implementation-roadmaps.md)**
- **[E7A Automation: универсальная внутренняя автоматизация и Draft PR во всех затронутых репозиториях](engineering-automation.md)** — `governance-sync` первый модуль; [пример отключённой конфигурации](../contracts/automation-modules.example.json)
- [Изначальный узкий план Governance Sync — теперь архив](governance-sync-service.md)
- [GitHub App: стоимость, ресурсы и безопасный запуск](github-app-cost-and-resources.md)

- **[Roadmap E7A — Governance → F0 → F1 → F2 → M1 и портфель продуктов](roadmap.md)**
- **[Жизненный цикл нового продукта: обязательные checklist и evidence gates](product-lifecycle-checklists.md)**
- [Шаблон Product Charter](templates/product-charter.md) · [шаблон Release Gate и evidence](templates/release-gate.md)
- **[Автоматическое распространение утверждённых стандартов в Draft PR](standards-propagation.md)** — концепция self-hosted механизма, **не запущен**
- **[Реестр опубликованной версии стандарта](standards-release.json)** — неизменяемый baseline SHA, не executable automation
- [Governance 2.1.0 — impact/migration and review evidence](releases/2.1.0.md)

- **[Финальный архитектурный baseline: восемь заключительных решений и устранение противоречий](final-architecture-baseline-2026-10-27.md)**
- **[Решения владельца: раунды 1–10, 2026-10-27 — актуальный архитектурный baseline](owner-decisions-2026-10-27.md)**
- [Games — самостоятельное продуктовое направление](games-product-direction.md)
- [Gateway — самостоятельный продукт управления семейным WireGuard-доступом](gateway-product-direction.md)
- [Философия и принципы](ecosystem-charter.md)
- [B2C-first стратегия и первая измеримая цель M1](growth-strategy-and-first-milestone.md)
- [Организационная структура и минимальный набор ролей/FTE](team-and-capacity.md)
- [Брендбук: исходная айдентика и её применение](../brandbook/README.md)
- [Собственный стикерпак E7A — MVP на 12 сюжетов](../brandbook/stickerpack.md)
- [Юридическая дорожная карта России (НПД/ИП/ООО, IP, software и соблюдение требований)](../legal/README.md)
- [Фирменная продукция и обязательные референсы худи](../brandbook/merchandise-and-hoodie.md)
- [Россия: практическая подача E7A на товарный знак и доходы при официальной работе](../legal/trademark-filing-and-employed-income-russia.md)
- [Россия: организационно-правовая форма и налоги](../legal/business-structure-russia.md)
- [Россия: товарный знак E7A, промышленный образец и права на стикеры](../legal/brand-ip-and-patents-russia.md)
- **[Выбранная стратегия E7A: одна комбинированная заявка логотип + название, класс 42](../legal/e7a-combined-mark-first-filing-strategy.md)**
- [E7A: предварительный поиск Роспатента, EA7 Armani, риск класса 25](../legal/trademark-clearance-e7a-preliminary-2026-10-25.md)
- [Россия: цифровые продукты, privacy, payments, hoodies/мерч](../legal/product-compliance-russia.md)
- [Матрица владения и данные](ownership-and-data.md)
- [Website: ценностное предложение, B2B/B2C и публичные маршруты](market-positioning-and-entrypoints.md)
- [Laboratory: пути разработчика, партнёра и сотрудника (B2D/B2E)](developer-and-partner-lifecycle.md)
- [Demo: витрина, песочницы и пробные сценарии](demo-and-trials.md)
- [Laboratory, три Kit, QA и выпуск](product-factory-and-kits.md)
- [Identity, Trust, SSO и доступ к продуктам](identity-and-trust.md)
- [Platform: СГДС, Jobs, коннекторы, Mail](platform-delivery-jobs-integrations.md)
- [Реальные и иллюстративные сценарии](scenarios.md)
- [Infra: профессорские серверы и эксплуатация](infra-operations.md)
- **[Единый стандарт зависимостей: Core / Product Starter / продукты / Automation](dependency-ownership.md)** — утверждён Owner E7A как расширение **2.1.0**; действует нормативно после отдельного approved immutable `standards-release.json` с полным SHA. Независимые consumer adoption PR, никакого автоматического CI/PR writer/deploy.
- [Версии стандартов и управление изменениями](standards-and-change-control.md)
- [Реестр архитектурных решений и открытых вопросов](decision-log.md)
- [Architecture Consistency Report — противоречия, риски, проверки](architecture-consistency-report.md)
- [Impact Matrix — репозитории, зависимости, версии стандартов](repository-impact-matrix.md)
- [10 практических задач и M1-гейты](implementation-priorities.md)

### Контрактные примеры (пока не runtime API)

- [Product Manifest — пример](../contracts/product-manifest.example.yaml)
- [Пакет интеграционных операций — пример](../contracts/operation-batch.example.json)
- [Подключение продукта к организационному стандарту — пример](../contracts/standards-consumer.example.json)
- [Манифест capabilities коннектора — пример](../contracts/connector-capabilities.example.json)
- [Evidence проверки релиза — пример](../contracts/release-evidence.example.json)

**Правило достоверности:** помечать отдельно APPROVED-CONCEPT, PROPOSED, IMPLEMENTED-SOURCE, VERIFIED-TEST и RUNNING-VERIFIED. PR, README, npm manifest, образ контейнера или готовый CLI сами по себе не являются доказательством production runtime.

**Исторический перечень Draft на момент первоначального аудита (PR #1 уже объединён):** [Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1), [.github #1](https://github.com/E7A-RU/.github/pull/1). Draft ≠ принято в main. Документацию синхронизировать через контролируемые PR, не автосливать.

## Смысл утверждения Governance v1

Владелец E7A подтвердил: слияние PR #1 после отдельного разрешения утверждает **первичные организационные и архитектурные стандарты**, а не готовность общих сервисов или реализаций продуктов. Канонический владелец и окончательный утверждающий изменений обязательных стандартов — **Founder / Owner E7A**. Исполнение и локальные технические решения могут делегироваться в границах утверждённых правил. [Управление изменениями](standards-and-change-control.md).


**Version decision:** Owner Decision 064 selects **MAJOR Governance 3.0.0** as the *next* release because resource authorization authority changes. This branch is still a source proposal; actual approved normative baseline stays **2.1.0** until separate reviewed descriptor publication after merge of the exact normative source SHA. Do not update consumers to this branch or mutable `main`.
