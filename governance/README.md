# E7A Governance — единый источник архитектурных решений

**Status: approved conceptual baseline as of 2026-10-25; proposed in Draft PR, not yet merged into main.** Никаких сервисов, автоматических деплоев или глобального входа само наличие файлов не создаёт.

## Иерархия источников истины

1. **Этот репозиторий, governance/** — организация, инженерная философия, общие границы, контракты интеграции между системами, политика безопасности и порядок решений.
2. **E7A-RU/core** — исполняемые исходники версионированных SDK, UI, типизированных server contracts и текущего опционального Product Starter.
3. **E7A-RU/laboratory** — реализация Wizard, QA, release evidence, approvals, реестр продуктовых потребителей стандартов.
4. **E7A-RU/infra** — реализация декларативной инфраструктуры, серверов, runtime, мониторинга, резервного копирования и deployment.
5. **Identity / Platform (планируемые самостоятельные продукты)** — свои модели, контракты, runtime и данные после отдельных ADR.
6. **Каждый продукт** — собственные схемы, процессы, бизнес-логика, доменные политики и релизные материалы.

Общий стандарт определяет **что и зачем**, продуктовый репозиторий — **как реализовано у него**. Локальные документы дополняют, но не ослабляют организационные политики. При несовместимости обязательна запись о конфликте и отдельный ADR; не менять десятки README без контроля версии.

## Документы

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
- **[E7A: предварительный поиск Роспатента, EA7 Armani, риск класса 25](../legal/trademark-clearance-e7a-preliminary-2026-10-25.md)**
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
- [Версии стандартов и управление изменениями](standards-and-change-control.md)
- [Реестр архитектурных решений и открытых вопросов](decision-log.md)

### Контрактные примеры (пока не runtime API)

- [Product Manifest — пример](../contracts/product-manifest.example.yaml)
- [Пакет интеграционных операций — пример](../contracts/operation-batch.example.json)
- [Подключение продукта к организационному стандарту — пример](../contracts/standards-consumer.example.json)
- [Манифест capabilities коннектора — пример](../contracts/connector-capabilities.example.json)
- [Evidence проверки релиза — пример](../contracts/release-evidence.example.json)

**Правило достоверности:** помечать отдельно APPROVED-CONCEPT, PROPOSED, IMPLEMENTED-SOURCE, VERIFIED-TEST и RUNNING-VERIFIED. PR, README, npm manifest, образ контейнера или готовый CLI сами по себе не являются доказательством production runtime.

**Связанные Draft:** [Core #25](https://github.com/E7A-RU/core/pull/25), [Website #57](https://github.com/E7A-RU/website/pull/57), [Reader #7](https://github.com/E7A-RU/reader/pull/7), [Bot #6](https://github.com/E7A-RU/bot/pull/6), [Infra #1](https://github.com/E7A-RU/infra/pull/1), [Laboratory #1](https://github.com/E7A-RU/laboratory/pull/1), [.github #1](https://github.com/E7A-RU/.github/pull/1). Draft ≠ принято в main. Документацию синхронизировать через контролируемые PR, не автосливать.
