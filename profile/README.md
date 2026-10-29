<div align="center">
  <img src="./assets/hero.svg" alt="E7A — технологический фундамент, независимые продукты" width="100%" />
</div>

# E7A — экосистема самостоятельных цифровых продуктов

**Engineering clarity · Product independence · Human control.**

Мы проектируем и развиваем полезные B2C-продукты, а также решения для автоматизации B2B. В основе — единые инженерные принципы, дизайн-система и проверяемые процессы выпуска, а не один универсальный backend.

[**Сайт E7A**](https://e7a.ru) · [**Governance**](../governance/README.md) · [**Roadmap**](../governance/roadmap.md) · [**Как мы запускаем продукты**](../governance/product-lifecycle-checklists.md)

## Принципы E7A

1. **Один стандарт, независимые продукты.** У каждого продукта свой код, доменные данные, ответственность за доступ и релизы; общий компонент не становится владельцем чужой БД.
2. **Core — инженерный фундамент, не работающий backend.** UI, типизированные контракты, SDK, дизайн-система и необязательный Product Starter.
3. **Foundation — общие возможности с чёткими границами.** Identity, Platform (Delivery/СГДС, Jobs, Connectors, Results), Audit, Billing Foundation, Laboratory и Infra развиваются независимо и подключаются по необходимости.
4. **Privacy/security by design.** Минимальные полномочия, изоляция пользователей и организаций, проверенные миграции и восстановление. Анонимные и локальные приложения не обязаны использовать Identity.
5. **Evidence before claims.** Проектирование ≠ исходники ≠ успешные тесты ≠ работающий сервис. Production требует отдельного согласования владельца.
6. **Права и бренд.** Официальные знаки и визуальный стиль E7A не предоставляются для стороннего брендинга без предварительного письменного разрешения. [Brand Policy](../BRAND_POLICY.md); наличие публичного npm-пакета не означает предоставления лицензии на код или бренд.

## Карта экосистемы

| Компонент | Назначение | Подтверждённый уровень / направление |
| --- | --- | --- |
| [Website](https://github.com/E7A-RU/website) · [e7a.ru](https://e7a.ru) | Публичная витрина, B2C-открытие продуктов, B2B-заявки и демо | Действующий сайт; новые сценарии внедряются отдельно |
| [Core](https://github.com/E7A-RU/core) | Версионированные UI, server contracts и Starter | Публичный `@e7a-ru/ui@0.2.1` опубликован; другие пакеты/версии — независимо |
| [Reader](https://github.com/E7A-RU/reader) | Локальное приватное чтение документов в браузере | MVP в исходниках; production-развёртывание не подтверждено |
| [Bot Studio](https://github.com/E7A-RU/bot) | Конструктор диалогов/бизнес-логики, собственный Bot runtime | Существующий runtime, развитие; общие коннекторы будут мигрировать по отдельным PR |
| [Planner](https://github.com/E7A-RU/planner) | Рабочие графики, задачи и календари | Дизайн/Starter в Draft, scope MVP отдельно утверждается |
| [Gateway](https://github.com/E7A-RU/gateway) | Управление персональным WireGuard-доступом | Разработка Control Plane; production Gateway не подтверждён |
| [Games](../governance/games-product-direction.md) | Направление браузерных и небольших мобильных игр | Зарегистрировано как направление; будущая витрина, игры не анонсированы |
| [Identity](https://github.com/E7A-RU/identity) | Person/Accounts/SSO/OIDC, организации и права доступа | Foundation направление; реализация не подтверждена |
| [Platform](https://github.com/E7A-RU/platform) | Delivery/СГДС, trusted Jobs, Connectors, Results | Foundation направление; реализация не подтверждена |
| [Laboratory](https://github.com/E7A-RU/laboratory) | Product Factory: Wizard, три Kit, QA, Sandbox и Release Gate | Архитектура; работающий портал не подтверждён |
| [Infra](https://github.com/E7A-RU/infra) | Среды, секреты, резервирование, наблюдаемость и контролируемые релизы | Архитектура; общая Infra-платформа не подтверждена |
| Audit / Billing Foundation | Централизованный защищённый аудит и коммерческие контракты | Отдельные Foundation-направления, не заявлены работающими |

> Часть репозиториев **приватные**: ссылки на них доступны участникам E7A с разрешённым доступом. Отсутствие публичного доступа не означает, что продукт запущен или доступен для заказа.

## Как мы реализуем продукты

**Три утверждённых Product Kit:** [Browser-only](../governance/kit-implementation-roadmaps.md#1-browser-only-kit), [Full-stack](../governance/kit-implementation-roadmaps.md#2-full-stack-kit) и [Service/Worker](../governance/kit-implementation-roadmaps.md#3-serviceworker-kit). Для stateful backend стандартный выбор — **PostgreSQL-first** с продуктовой БД и проверяемыми SQL-миграциями; без принудительного backend для browser-only.

**Этапы:** Governance → Foundation F1 (проверенная интеграция) → F2 (восстановление/эксплуатация) → M1 (реальное использование первого B2C-продукта). Это цели, а не заявления, что всё уже работает.

[Roadmap](../governance/roadmap.md) · [Backend reference architecture](../governance/backend-reference-architecture.md) · [Product Charter](../governance/templates/product-charter.md) · [Release checklist](../governance/templates/release-gate.md)

## Как мы работаем с изменениями

Организационные правила принимает владелец E7A. Они версионируются в [.github/governance](../governance/README.md); каждому затронутому репозиторию отдельный Draft PR предлагает безопасную адаптацию. Никакого неконтролируемого массового merge или production deployment. **GitHub-hosted Actions CI не используем**; предусмотрено только отдельно разрешённое применение Actions для выпуска пакетов Core.

**Контакты и сотрудничество:** [e7a.ru](https://e7a.ru). Доступ к внутренним инженерным инструментам и репозиториям предоставляется по отдельному согласованию.

---
*E7A · Системность, безопасность и уважение к вниманию.*
