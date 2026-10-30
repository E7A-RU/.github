# E7A Product Stewardship — единый порядок сопровождения репозиториев

**Статус:** организационный процесс на основе уже утверждённого [Governance 1.1.0](standards-release.json). Эта запись конкретизирует ответственность ВП/инженерного агента и владельца продукта, **не** выдаёт права на production, не является новым runtime или отдельным опубликованным нормативным релизом. Утверждённая ревизия: `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`.

## 1. Границы ответственности

| Роль | Может | Не получает автоматически |
| --- | --- | --- |
| **Owner E7A / accountable human** | Утверждает обязательные общие правила, продуктовые решения, исключения, production-релизы и делегирование | Пройденный QA только от факта собственного разрешения |
| **ВП / проектный инженерный агент** | Работает в одной продуктовой ветке/PR, поддерживает документы, выполняет локальные тесты, оформляет release evidence, предлагает ADR и изменения | Право менять чужой product main, App scopes, секреты, инфраструктуру, production или обязательный Governance |
| **Product Owner / делегированный ответственный** | Принимает scope продукта, поддерживает roadmap, критерии готовности, review изменений | Автоматическое утверждение изменений базовой политики E7A или production |
| **QA reviewer / Laboratory (когда реализованы)** | Проверяет exact-head команды и результаты, применимые негативные сценарии, готовит evidence | Статус реального успешного QA без запуска; production apply |
| **Infra operator** | Проверяет отдельную действительную owner-approved release-запись, осуществляет контролируемый apply/rollback | Управление продуктовой бизнес-логикой, Identity Person или Billing |
| **E7A Automation (будущий инструмент)** | Обнаруживает несогласованность, формирует ограниченные *Draft PR proposals* с объяснением | Прямой push в main, бесконтрольный merge, произвольные AGENTS-инструкции или deploy |

При одном человеке многие роли могут временно совмещаться; это **документированное исключение**, а не заявление о независимом многопользовательском review. Назначения конкретных людей/агентов должны быть внесены в product-local maintainership, **не придумываются** этим документом.

## 2. Канонический минимум в каждом репозитории

- `README.md`: ясное назначение продукта/компонента, границы, ссылки на нормативные стандарты, честный статус, способы локального запуска там, где они реально существуют.
- `governance-consumer.json`: проверяемый `schemaVersion=e7a.governance-consumer/v1`, точная версия и **immutable SHA** текущего принятого стандарта; **не** свидетельство реализации всех требований.
- `docs/MAINTAINERSHIP.md`: передача контекста ВП, допустимые работы, владельцы/незаполненные назначения, активные PR, QA и release gates, ограничения, следующие задачи.
- `AGENTS.md`: безопасность, ограничения полномочий и способ локальной проверки. Если документ пока находится **только в большом Draft PR**, это отражается в `docs/MAINTAINERSHIP.md`; не перезаписывать его автоматически поверх ветки другого агента.
- Документ о продуктовых/доменных границах (Product Charter, ADR, статус) — нужен с учётом Kit и этапа продукта. Для непустого stateful backend базовая рекомендация — [PostgreSQL-first](backend-reference-architecture.md); Browser-only **не** обязан иметь БД.
- Ссылки на [три Kit](kit-implementation-roadmaps.md), [Product Lifecycle](product-lifecycle-checklists.md), [Roadmap](roadmap.md), [Brand Policy](../BRAND_POLICY.md) и локальные инженерные runbooks.

**При пустом репозитории:** только разрешённый **однократный начальный bootstrap** README в `main`, затем новые файлы через отдельный PR. Bootstrap не является созданием/деплоем Identity или Platform.

## 3. Правила ВП/агента при начале сессии

1. Прочитать `AGENTS.md` в `main` (или в текущем рабочем PR, если main ещё не содержит), `docs/MAINTAINERSHIP.md`, локальный README и канонический Governance.
2. Получить **живые** `main` SHA, список PR и актуальный head PR. Исторические SHA в документации не считать текущими без проверки.
3. Продолжать уже существующий PR на ту же задачу; не создавать конкурирующие ветки и не переписывать работу другого агента. Для новой независимой задачи — отдельная ветка/PR.
4. Определить применимость Browser-only / Full-stack / Service/Worker и точное тестовое окружение; **ни один** Kit не предполагает общий обязательный backend-hop.
5. Для функционального изменения: frozen install, typecheck, lint/build/tests/negative/integration, применимые backup/restore/browser/security checks **на точном SHA**. GitHub-hosted Actions CI запрещён.
6. Для docs-only: проверить links/JSON/config/diff, отсутствие опасных файлов и противоречий с **уже работающим продуктом**. При незавершённом runtime QA указать `NOT_TESTED`, не PASS.
7. Отдельно фиксировать 1) merged source SHA, 2) source branch/head, 3) действительно установленный/запущенный artifact, 4) опубликованный package и 5) проверку production — это **разные факты**.
8. Перед завершением обновить свой PR/maintainership: изменения, точный head, результаты, blockers, следующий инженерный шаг и что требует решения владельца.

## 4. Режим PR и разрешения

- **Draft PR** разрешён и предпочтителен для разработки; он **не** равен approved release.
- **Docs-only merge** возможен после owner-директивы, полного просмотра diff, проверки ссылок/реестра и отсутствия runtime/cron/deploy side effects. Источник не объявляется протестированным, если команды не запускались.
- **Code, Starter CLI, dependency/lockfile, SQL migrations, privileged helper, identity/auth** требуют воспроизводимого exact-SHA тестирования и соответствующего инженерного review. `mergeable: true` от GitHub означает отсутствие Git-конфликтов, **не готовность**.
- **GitHub-hosted Actions CI запрещён**; единственное отдельно разрешаемое исключение — ручной package publisher Core. Не создавать Actions workflow даже ради улучшения проверок.
- **Автоматическое создание Draft PR** E7A Automation допускается только после отдельной проверки read-only и owner approval на App + конкретную write canary. Нельзя создавать PR массово на основании произвольных слов в README.
- **Production** — отдельная утверждаемая запись: product, SHA, digest, environment, QA/recovery/rollback, owner approval и Infra validation. Docs/source merge не вызывает deploy.
- Изменение обязательного Governance — отдельное owner-approved решение/ADR. Product-local ADR не ослабляет его требования.
- Бренд E7A и официальный визуальный стиль **не разрешены** к стороннему использованию без согласования с правообладателями.

## 5. Единая шкала доказательств

`DIRECTION / DESIGN → IMPLEMENTED_SOURCE → TESTED_EXACT_SHA → STAGING_VERIFIED → OWNER_APPROVED_FOR_PROD → RUNNING_VERIFIED`.

`UNKNOWN`, `SKIPPED`, `BLOCKED` не приравниваются к `PASS`. Статус `Governance pin matches` означает только версию правил в репозитории. Никогда не считать наличие `package.json`, SQL миграций, контейнера или PR доказательством готовности остальных стадий.

## 6. Контрольный цикл каждого продукта

**При каждом PR:** обновить scope, acceptance, exact head, affected paths, QA, security impact, owner decision. **Еженедельно:** проверить открытые PR, блокеры, зависимости, полезность/здоровье продукта и потребность в patch. **Перед релизом:** отдельная проверка, recovery/rollback, approval. **При изменении Governance:** E7A Automation предлагает targeted Draft PR, владелец/ВП продукта проводит review.

Product-local шаблон: [MAINTAINERSHIP](../governance/templates/product-maintainership.md). [Reconciliation sprint ledger](consumer-stewardship-sprint-2026-10-30.md) фиксирует статус начального внедрения, а не автоматически обновляемую систему.

## 7. Границы при конкретных продуктах

- **Website:** до любого source-merge/deploy проверять старый cron и точный approval SHA; наличие здорового контейнера и `main` в GitHub не тождественны.
- **Reader:** сохранить anonymous/local-first работу и отсутствие неожиданной загрузки файлов.
- **Bot:** текущие PostgreSQL/RabbitMQ/адаптеры принадлежат Bot до независимой миграции в Platform.
- **Gateway:** не менять живой WireGuard/S-UI/Butlerov/MikroTik/VDS без отдельного разрешения; code-only PR с helper и PostgreSQL не значит running.
- **Infra/Laboratory:** QA Sandbox не production staging; Infra выполняет разрешённый apply, Laboratory готовит доказательства.
- **Planner:** продуктовая область и MVP пока не утверждены; Draft Starter не означает готовый рабочий продукт.
- **Automation:** подготовка Draft PR — не возможность автоматически их объединять; read-only source не равен зарегистрированной App.
- **Identity/Platform:** пустые или начальные репозитории не обозначают развернутый общий сервис.

**Canonical:** [Governance Index](README.md) · [Approved release](standards-release.json) · [Product Checklist](product-lifecycle-checklists.md) · [Standards propagation](standards-propagation.md).
