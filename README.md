# E7A — документация организации

Публичные архитектурные правила, профиль организации и материалы бренда E7A.
**Навигация сверена 18 ноября 2026 года.** Статус опубликованного стандарта,
его принятие продуктом и готовность работающего сервиса учитываются отдельно.

| Задача | Точка входа |
| --- | --- |
| Познакомиться с E7A и продуктами | [Публичный профиль](profile/README.md) · [e7a.ru](https://e7a.ru) |
| Найти организационные правила | [Governance](governance/README.md) |
| Проверить опубликованную версию стандарта | [Release descriptor](governance/standards-release.json) · [история 4.0.0](governance/releases/4.0.0.md) |
| Понять границы продуктов и общих компонентов | [Ecosystem Charter](governance/ecosystem-charter.md) · [Dependency ownership](governance/dependency-ownership.md) |
| Начать или продолжить продуктовую работу | [Product Stewardship](governance/product-stewardship.md) · [Lifecycle](governance/product-lifecycle-checklists.md) · [шаблон передачи](governance/templates/product-maintainership.md) |
| Найти направление развития | [Roadmap](governance/roadmap.md) · [M1 Product-in-Use](governance/growth-strategy-and-first-milestone.md) |
| Использовать фирменные материалы | [Brand Usage Policy](BRAND_POLICY.md) · [Brandbook](brandbook/README.md) |
| Изучить юридические исследования | [Индекс исследований](legal/README.md); проверять дату и актуальность каждого документа |

## Опубликованный стандарт и его потребители

В [descriptor](governance/standards-release.json) опубликован **Governance 4.0.0**:
нормативный исходный SHA `abba7788f305bbf4d0494d8b34c82aeea197a2ee`,
отдельный publication commit
[`5c11fd53dc3b203ce5166187cc10d564ad3f2c1d`](https://github.com/E7A-RU/.github/blob/5c11fd53dc3b203ce5166187cc10d564ad3f2c1d/governance/standards-release.json).
Это адресное изменение процедуры работы с исходниками Identity.
Оно не обновляет автоматически pins остальных продуктов, их зависимости или runtime.

Текущую принятую версию продукта проверяют в его `governance-consumer.json`
и документации adoption. Документы на `main` удобны для навигации;
нормативные ссылки фиксируются на неизменяемой ревизии.

[ECOSYSTEM_ARCHITECTURE.md](ECOSYSTEM_ARCHITECTURE.md) и
[срез портфеля 31.10.2026](governance/portfolio-status-2026-10-31.md) —
исторические документы. [Публичная сверка 18.11.2026](governance/portfolio-status-2026-11-18.md)
уточняет сведения профиля, не выпускает новый стандарт.

## Core и независимые продукты

Core — версионируемые UI-библиотеки, нейтральные server contracts и необязательный
Product Starter. Identity, Platform, Infra, Laboratory и продукты сохраняют
свои границы ответственности. [Три Product Kit](governance/kit-implementation-roadmaps.md)
не требуют общего backend для browser-only приложения.

Публичные npm-метаданные подтверждают **`@e7a-ru/ui@0.5.0`**
на 18.11.2026: [реестр](https://www.npmjs.com/package/@e7a-ru/ui/v/0.5.0).
Это не версия каждого продукта. Adoption требует отдельного PR, точного pin,
lockfile и проверки потребителя. [Принятая политика поддержки](CORE_UI_SUPPORT.md): 0.5.0 — текущая,
0.4.0 — предыдущая с ограниченной поддержкой, 0.3.x и старше — архивные,
неподдерживаемые. Окно — текущий и один предыдущий minor; максимум два
предыдущих по отдельному временному решению владельца. Действующий минимум —
0.4.0. Исторические опубликованные пакеты остаются неизменяемыми.

## Работа участников

Для участников предусмотрен отдельный приватный репозиторий
`.github-private` с `profile/README.md`: карта репозиториев, ссылки на handoff,
датированный source-аудит и незавершённая работа.
Публичный профиль обслуживает `.github/profile/README.md`.
[Как GitHub разделяет представления](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile).

Одна задача — одна рабочая ветка — один PR в соответствующем репозитории.
Сначала проверить текущую работу; при продолжении сохранять уникальные изменения
и историю. В PR указать владельца, цель, статус, критерии готовности,
проверенный SHA, команды, место запуска, логи и следующий шаг.

GitHub Actions CI/deploy запрещены. Проверки выполняются локально или на
Lobachevsky. Исключение — только отдельно разрешённый ручной package publisher
Core; он не является продуктовым CI. Merge и production — отдельные решения.

## Права и публичность

Бренд E7A, логотипы и фирменные материалы требуют предварительного письменного
разрешения правообладателей для стороннего использования.
Публичная загрузка UI (`UNLICENSED`) не предоставляет прав на код или бренд.
Секреты, приватные операционные сведения и внутренние отчёты сюда не добавляются.
