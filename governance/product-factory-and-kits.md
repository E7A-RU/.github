# Laboratory: Wizard + QA + Experiments + Release Gate

**Статус:** целевая концепция Laboratory согласована; существующая реализация не доказана как работающий Developer Portal. Laboratory — **продукт для производства продуктов**, а не монолитный backend и не балансировщик.

## Четыре функции Laboratory

**1. Wizard / Create.** Пользователь задаёт: название и slug, назначение, тип Kit, домен (без немедленного DNS apply), public/private access, необходимость Identity, Platform capabilities, БД/очередей, ресурсы. Wizard генерирует spec и вызывает опциональный существующий **Core Product Starter** по согласованной версии. Не копировать шаблоны в Laboratory. Каждому продукту свой Git-репозиторий.

**2. QA / Validate.** Проверки source policy, lint/typecheck, tests, package audit, contracts, browser E2E где применимо, security/negative scenarios, контейнерный build/smoke, конфигурации и миграций. Исполнение на изолированных runner Infra с CPU/RAM/disk/network budgets, без production secrets, с логами exact SHA и hash артефакта. Не объявлять failed/partial checks pass.

**3. Experiment / Sandbox.** Повторяемые временные окружения для тестов и прототипов. Infra выделяет worktree/container/networks, owner-controlled cleanup; логи остаются. Рабочие команды/секреты/тома production не монтировать внутрь PR runner.

**4. Release Gate.** Собирает release manifest, status checks, risk/rollback/approvals, provenance и evidence. **Готовность к выпуску ≠ разрешение на production apply**. Laboratory направляет одобренный релиз в Infra, Infra исполняет и измеряет здоровье.

## Три профиля Kit (целевые)

| Kit | Что включено | Где backend | Пример |
| --- | --- | --- | --- |
| Browser-only | FE (например Next.js), клиентские операции, health/smoke | Не требуется; backend features опциональны | Reader читает PDF/DOCX/EPUB/FB2 локально, не отправляя книги на сервер |
| Full-stack | FE + собственный API, optional DB/cache/worker | Product-owned | Planner: задачи в своей БД, Identity для логина |
| Service / Worker | API/consumer/worker, optional UI и брокер | Product-owned или Platform capabilities для узкого сервиса | Bot Studio API+RabbitMQ+работники, интеграционный worker |

**Core** владеет общими версиями SDK и текущим опциональным Starter; отдельные реализованные версии всех 3 Kit ещё требуют реализации и проверок. При выделении generator в новый repo сначала ADR/owner approval; не делать скрытый перенос.

## Иллюстративный Wizard flow

Назовём продукт analytics.e7a.ru, Kit Full-stack; требуется E7A Identity и собственный PostgreSQL, Redis нет, workers нужны только после измеренного спроса. Wizard формирует **не исполняемый напрямую** декларативный manifest с продуктом, образом, healthcheck, tenancy и проверками. Infra делает план ресурсов и DNS/TLS; Laboratory проверяет собранный digest и exact SHA; владелец разрешает release. См. [пример manifest](../contracts/product-manifest.example.yaml).

## PR-driven QA на Lobachevsky — из нашей эксплуатационной практики

Идея уже обсуждалась для KS54 (черновой прототип PR #761): PR → временный Git worktree → контейнеризированные проверки → локальный immutable log → статус/комментарий в PR → после merge/close cleanup worktree. **Это не значит, что новый runner E7A установлен.** Важно отдельно проверить блокировки, неизменяемый head SHA, лимиты CPU/RAM, disk caps, защиту production Docker socket, своевременное удаление и сохранение audit trail. Проверки GitHub-hosted Actions запрещены текущей политикой E7A; запускать self-hosted/local.

## Результат QA и разрешение на применение

Минимум: product/repo, commitSha, artifact digest, kit/schema version, standards revision, check suite version, server/runner identity (без секрета), start/end, log reference, risk, DB migration/backup evidence, approval subject и expiry, rollback image/tag и health outcome. См. [release evidence](../contracts/release-evidence.example.json).

Статус: PROPOSED → VALIDATED → APPROVED → DEPLOYING → VERIFIED или ROLLED_BACK/FAILED. Отдельно хранить SKIPPED (запуск пропущен из-за lock), не выдавать skipped за green.

## Граница с Identity и Platform

Identity доказывает, кто инициировал QA/release и какие у него общие права. Laboratory хранит спецификацию и decision record. Platform (если появится) может обеспечить доставку уведомления о результате, но не решает, **можно** ли выпускать релиз; Infra применяет production изменения только с отдельным действующим разрешением.

## Безопасность

Тестовые запуски не имеют access к prod PostgreSQL, секретам, сети внутренних admin API и Docker socket без изоляции. Secrets delivery — серверная зона Infra, не pipeline YAML с токенами. Нет автоматического merge, package publication или production push вследствие создания репозитория.
