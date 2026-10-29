# Infra — эксплуатационный слой E7A

**Статус:** концепция согласована; общий provisioner ещё не развёрнут и не проверен.

## Функция Infra

Физические хосты и ВМ, Docker Compose, сети, DNS/TLS, секреты, выделение CPU/RAM/disk, monitoring, DB hosting, backup/restore, QA runner и контролируемые deployments. **Владение размещением БД не означает владение её схемой и данными.** Данные Identity, Platform и Bot остаются у их владельцев.

Узлы именуются фамилиями профессоров/учёных: первый — Lobachevsky. Перемещение продукта на другой узел не должно менять его публичный service identity или API.

## Почему пока Compose, а не Kubernetes/Proxmox

Один физический узел с 12 ядрами, ~31 GiB RAM и ~433 GiB disk не получает новых ресурсов при добавлении гипервизора или K3s. ВМ полезны для дополнительной изоляции; K3s — при росте многоузловой оркестрации. Первый этап — раздельные Compose projects, networks, service accounts, quotas и единый управляемый reverse proxy. Kubernetes включать только после cost/security/operations ADR.

## Declarative product deployment

1. Laboratory отправляет product manifest без секретов.
2. Infra проверяет schema, host capacity, namespace/port/domain collisions, разрешённый ingress/egress, dependencies, volumes и backup requirements.
3. Infra публикует diff желаемого/текущего состояния, назначение ресурсов и план отката. Никакого apply на основе README.
4. Staging создаётся в пределах выданных policy/grants и quotas.
5. Owner подписывает **конкретный** release manifest/digest, target host и окно.
6. Infra применяет, проверяет healthchecks, сохраняет журнал, status и rollback. Изменения PostgreSQL и других stateful систем требуют отдельной проверки совместимости миграции и восстановления.

[Пример product manifest](../contracts/product-manifest.example.yaml) — иллюстрация, не действующий CRD.

## Базы/DR/безопасность

Раздельные DB roles, network сегментация, TLS, least privilege, секреты server-owned. Допустимо физически использовать один Postgres с независимыми DB/roles после оценки blast radius. Для критичных Identity/SGDS нужны off-host backups, тесты восстановления, RPO/RTO, budgets и независимые меры availability; snapshots на том же диске недостаточны.

## Практический пример Lobachevsky — Docker GC

В KS54 удаление только dangling images не охватывало tagged releases. Перед изменением проверили systemd timer, deploy flock и Docker images. Dry-run отобрал 57 кандидатов; при контролируемом apply 54 удалены, 3 Docker отклонил из-за нескольких tags (без force). BuildKit cache pruned отдельно; свободный диск вырос примерно с **97 GiB до 165 GiB**, критические контейнеры были healthy. Текущие данные — историческая диагностика, не вечная метрика. Эксплуатационный урок: защищать running/stopped container IDs, current/previous rollback, shared layers, volumes, production DB; не использовать слепой system prune with volumes. Различать EXECUTED, SKIPPED_LOCK и WARN; сохранять журнал GC. Не копировать KS54 скрипт на E7A без оценки других проектов.

## Self-hosted CI

Временные worktree/контейнеры для PR, immutable head SHA, CPU/RAM/disk лимиты, isolation Docker socket, полные логи, подтверждение результатов и cleanup после merge. Наличие чернового прототипа KS54 PR #761 — не production готовность E7A runner.

## Никакой несанкционированной автоматики

По умолчанию никакой git push или создание репозитория не разворачивает ВМ/DNS/базы. Laboratory организует проверку; Infra применяет строго разрешённый план. GitHub-hosted Actions CI запрещён.
