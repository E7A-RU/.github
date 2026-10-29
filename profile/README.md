<div align="center">

<img src="./assets/hero.svg" alt="E7A — engineering ecosystem with independent products" width="100%" />

# E7A

### Проектируем, разрабатываем и развиваем полезные цифровые продукты

**B2C-first · B2B automation · Engineering-first**

Единая система инженерных принципов — самостоятельные продукты, понятные границы ответственности и релизы, которые можно проверить и безопасно откатить.

[**Веб-сайт E7A**](https://e7a.ru) · [**Наши продукты ↓**](#продуктовая-линейка) · [**Governance**](https://github.com/E7A-RU/.github/tree/main/governance) · [**Roadmap**](https://github.com/E7A-RU/.github/blob/main/governance/roadmap.md)

</div>

---

## Что мы создаём

| **Продукты для людей** | **Автоматизация для бизнеса** | **Инженерный фундамент** |
| :--- | :--- | :--- |
| Удобные пользовательские инструменты, чтение, планирование, игровые гипотезы | Диалоги, боты, интеграции, повторяемые бизнес-процессы | Общая дизайн-система, типизированные контракты, безопасность, управляемая инфраструктура |

Мы не сводим экосистему к единственному серверу или универсальному backend. Каждый продукт сохраняет **свои данные, свой релиз и собственную ответственность**, а общие возможности подключает по контрактам тогда, когда они действительно нужны.


## Продуктовая линейка

### Пользовательские продукты и направления

| Продукт | Для чего | Текущий публично заявляемый этап |
| :--- | :--- | :--- |
| **[E7A Website](https://e7a.ru)** | Главная точка входа: каталог, знакомство с продуктами, будущие Demo и B2B-запросы | Существующий сайт; новые сценарии внедряются отдельно |
| **[Reader](https://github.com/E7A-RU/reader)** | Быстрое, приватное чтение своих документов прямо в браузере | Browser-first MVP в исходниках; публичный запуск не подтверждён |
| **[Bot Studio](https://github.com/E7A-RU/bot)** | Создание диалогов и сценариев бизнес-автоматизации | Существующий Bot runtime; Studio и общие интеграции развиваются |
| **[Planner](https://github.com/E7A-RU/planner)** | Рабочие графики, задачи, календари и планирование | Проектирование / Draft Starter; MVP ещё не утверждён |
| **[Gateway](https://github.com/E7A-RU/gateway)** | Контроль доступа устройств к личному WireGuard-шлюзу | TypeScript/PostgreSQL исходники в разработке; не подтверждён как запущенный продукт |
| **[Games](https://github.com/E7A-RU/.github/blob/main/governance/games-product-direction.md)** | Направление браузерных и небольших мобильных игр; будущая витрина | Зарегистрированное направление, без анонса готовой игры |

*Многие репозитории приватны: переход по ссылке доступен только участникам с правами. Указанный статус не является заявлением о доступности конкретной функции прямо сейчас.*

### Основа экосистемы

| Компонент | Зона ответственности | Фактическая граница |
| :--- | :--- | :--- |
| **[Core](https://github.com/E7A-RU/core)** | Дизайн-система, React UI, typed SDK/contracts, необязательный Product Starter | Не backend и не центральная БД. Публичный `@e7a-ru/ui@0.2.1` опубликован |
| **[Identity](https://github.com/E7A-RU/identity)** | Person / Account / Organization / Workspace, OIDC, доступ, проверенные claims | Репозиторий создан; работающий сервис не подтверждён |
| **[Platform](https://github.com/E7A-RU/platform)** | Delivery/СГДС, trusted Jobs, Connectors, Results | Самостоятельная Foundation capability, ещё не RUNNING |
| **[Laboratory](https://github.com/E7A-RU/laboratory)** | Product Factory: Wizard, три Product Kit, QA, Sandbox, Release Gate | Планируемый продукт, не универсальный backend |
| **[Infra](https://github.com/E7A-RU/infra)** | Инвентарь серверов, окружения, секреты, backup/recovery, контролируемые релизы | Отдельная эксплуатационная граница; общая платформа ещё не подтверждена |
| **[Central Audit / Billing Foundation](https://github.com/E7A-RU/.github/blob/main/governance/roadmap.md)** | Защищённый аудит; тарифы, заказы, подписки, entitlements | Утверждённые архитектурные направления, не готовые платежи |
| **[Engineering Automation](https://github.com/E7A-RU/.github/blob/main/governance/engineering-automation.md)** | Поддержка стандартов, подготовка PR, синхронизация документации и инженерные операции | Внутренний проект; отдельный runtime/App пока не созданы |

## Принципы разработки

| | Наш подход |
| :--- | :--- |
| **01 — Product ownership** | Каждый продукт владеет своим доменом, данными, ролями и жизненным циклом. Нет единой бизнес-БД для всех |
| **02 — TypeScript / PostgreSQL** | TypeScript-first; для stateful backend предпочтителен PostgreSQL с независимой продуктовой БД, проверяемыми SQL-миграциями и восстановлением |
| **03 — Privacy & security** | Отдельные субъекты Person / Account / SecurityActor, минимальные полномочия, независимые границы доверия, no secrets in Git |
| **04 — Reusable, not coupled** | Core выпускает пакеты и контракты; Identity, Platform, Infra и Laboratory не подменяют друг друга |
| **05 — Evidence before release** | Исходники ≠ пройденные тесты ≠ работающее production. Каждое утверждение о готовности требует подтверждения |
| **06 — Human control** | Изменения глобальных правил утверждает владелец; production без отдельного разрешения не меняется |

### Три пути создания нового продукта

| Kit | Типичная задача | Данные и runtime |
| :--- | :--- | :--- |
| **[Browser-only](https://github.com/E7A-RU/.github/blob/main/governance/kit-implementation-roadmaps.md)** | Reader, локальное приложение или браузерная игра | Работает в браузере; сервер, учётная запись и PostgreSQL **не обязательны** |
| **[Full-stack](https://github.com/E7A-RU/.github/blob/main/governance/kit-implementation-roadmaps.md)** | Продукт с собственными API и состоянием | TypeScript frontend/backend + продуктовая PostgreSQL, если требуется долговременное хранение |
| **[Service / Worker](https://github.com/E7A-RU/.github/blob/main/governance/kit-implementation-roadmaps.md)** | Интеграции, события, фоновые операции | Версионированные контракты, идемпотентность, retries, Results и PostgreSQL при durable state |

**Стандартный процесс:** идея → Product Charter → Kit и ADR → реализация → локальная/self-hosted проверка → staging → отдельное разрешение на production → наблюдение и следующий безопасный релиз.

[**Product Lifecycle Checklists**](https://github.com/E7A-RU/.github/blob/main/governance/product-lifecycle-checklists.md) · [**Backend Reference Architecture**](https://github.com/E7A-RU/.github/blob/main/governance/backend-reference-architecture.md) · [**Product Charter Template**](https://github.com/E7A-RU/.github/blob/main/governance/templates/product-charter.md)

## Roadmap: от фундамента к пользовательской ценности

```text
GOVERNANCE  ── единые архитектурные правила и границы ответственности
      ↓
FOUNDATION F1 ── проверенная интеграция необходимых общих capabilities
      ↓
FOUNDATION F2 ── восстановление, безопасность и эксплуатационная готовность
      ↓
M1 PRODUCT-IN-USE ── реальное использование + обратная связь + повторный релиз
```

Разработка Website и проверка B2C-гипотез происходят параллельно техническому Foundation. **Reader и Games — кандидаты**, а не автоматически назначенные первые флагманы. [Подробный Roadmap →](https://github.com/E7A-RU/.github/blob/main/governance/roadmap.md)

## Открытая инженерная документация

- [**Governance**](https://github.com/E7A-RU/.github/tree/main/governance) — организационные решения и границы компонентов
- [**Brandbook**](https://github.com/E7A-RU/.github/tree/main/brandbook) — визуальные принципы и концептуальные референсы
- [**Standards & change control**](https://github.com/E7A-RU/.github/blob/main/governance/standards-and-change-control.md) — версионирование глобальных правил, review, ADR и обновление потребителей
- [**E7A Automation**](https://github.com/E7A-RU/.github/blob/main/governance/engineering-automation.md) — будущая внутренняя автоматизация Draft PR без автоматического merge
- [**E7A Brand Usage Policy**](https://github.com/E7A-RU/.github/blob/main/BRAND_POLICY.md) — использование имени, логотипов и фирменных материалов сторонними лицами допускается только по предварительному письменному разрешению правообладателей; публичность библиотеки не предоставляет лицензию на исходный код

---

<div align="center">

**E7A · Технологии, которые можно понимать, проверять и развивать.**

[Сайт](https://e7a.ru) · [Roadmap](https://github.com/E7A-RU/.github/blob/main/governance/roadmap.md) · [Governance](https://github.com/E7A-RU/.github/tree/main/governance)

</div>
