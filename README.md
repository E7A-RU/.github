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

## Источники истины

| Информация | Где поддерживается |
| --- | --- |
| Опубликованный стандарт и нормативный SHA | [Release descriptor](governance/standards-release.json); пояснения в [Governance index](governance/README.md) |
| Принятая версия конкретного продукта | Его `governance-consumer.json` и adoption-документ |
| Поддерживаемые версии UI и условия миграции | [Core UI support](CORE_UI_SUPPORT.md) |
| Возможности и доступность продуктов | [Публичный профиль](profile/README.md); технические подтверждения — в документах продуктов |
| Внутренняя навигация и порядок работы | Приватный `.github-private/profile/README.md` |
| Датированные наблюдения | [Публичная сверка 18.11.2026](governance/portfolio-status-2026-11-18.md) |

README служат навигацией и краткими представлениями для разных аудиторий.
Таблицы версий, правила поддержки и эксплуатационные журналы ведутся в
указанных источниках; при их изменении здесь обновляются ссылки, а не копии правил.
Нормативное принятие продукта не следует автоматически из нового `main`.

[ECOSYSTEM_ARCHITECTURE.md](ECOSYSTEM_ARCHITECTURE.md) и
[срез 31.10.2026](governance/portfolio-status-2026-10-31.md) сохраняются как история.
GitHub Actions CI/deploy запрещены; проверки документации выполняются локально.

## Права и публичность

Бренд E7A, логотипы и фирменные материалы требуют предварительного письменного
разрешения правообладателей для стороннего использования.
Публичная загрузка UI (`UNLICENSED`) не предоставляет прав на код или бренд.
Секреты, приватные операционные сведения и внутренние отчёты сюда не добавляются.
