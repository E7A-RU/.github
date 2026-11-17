# Infra — эксплуатационный слой E7A

**Статус:** концепция согласована; общий provisioner ещё не развёрнут и не проверен.

## Функция Infra

Физические хосты и ВМ, Docker Compose, сети, DNS/TLS, секреты, выделение CPU/RAM/disk, monitoring, DB hosting, backup/restore, QA runner и контролируемые deployments. **Владение размещением БД не означает владение её схемой и данными.** Данные Identity, Platform и Bot остаются у их владельцев.

Узлы именуются фамилиями профессоров/учёных: первый — Lobachevsky. Перемещение продукта на другой узел не должно менять его публичный service identity или API.

## Почему пока Compose, а не Kubernetes/Proxmox

Один физический узел с 12 ядрами, ~31 GiB RAM и ~433 GiB disk не получает новых ресурсов при добавлении гипервизора или K3s. ВМ полезны для дополнительной изоляции; K3s — при росте многоузловой оркестрации. Первый этап — раздельные Compose projects, networks, service accounts, quotas и единый управляемый reverse proxy. Kubernetes включать только после cost/security/operations ADR.

## Declarative product deployment — минимальный самохостинговый путь

См. [release/deploy policy](release-deployment-policy.md). **Цель:** `protected main merge → self-hosted exact-SHA QA → immutable build → production healthcheck → rollback при ошибке`, без обязательного staging и второго ручного подтверждения каждого безопасного выпуска.

1. Product поставляет тесты, build/health/rollback contract и manifest **без секретов**. На раннем этапе достаточно локального `systemd timer` и отдельного Docker Compose project на Lobachevsky: отдельный сервер, общий Release Controller и Laboratory runtime не обязательны.
2. Infra проверяет host capacity, namespace/port/domain collisions, ingress/egress, права, dependencies, volumes, backup и blast radius. Никакого apply на основе README.
3. Владелец **один раз** утверждает ограниченный standing authorization (продукт, protected branch, R0/R1 allowlist, target, expiry/revocation, QA и rollback), а Infra отдельно проверяет и включает executor. Без такого activation сам merge в main ничего не разворачивает.
4. Для разрешённых изменений executor фиксирует exact SHA, проводит изолированные проверки без production secrets/privileged Docker socket, собирает и проверяет immutable digest; при любой ошибке не трогает production. Staging выполняется **только** по зафиксированному risk-trigger.
5. Infra переключает лишь выбранный сервис, фиксирует текущий/предыдущий digest, проверяет health/smoke и пишет журнал. На сбое делает rollback или объявляет `RECOVERY_REQUIRED` с остановкой повторных попыток.
6. Изменения R2+, IAM/секретов/сетей, критические DB migration и самого release controller **вне standing authorization** требуют отдельного решения Owner и дополнительных подходящих тестов/плана восстановления. Откат Docker-образа не является откатом БД.

[Пример product manifest](../contracts/product-manifest.example.yaml) — иллюстрация, не действующий CRD.

## Базы/DR/безопасность

Раздельные DB roles, network сегментация, TLS, least privilege, секреты server-owned. Допустимо физически использовать один Postgres с независимыми DB/roles после оценки blast radius. Для критичных Identity/SGDS нужны off-host backups, тесты восстановления, RPO/RTO, budgets и независимые меры availability; snapshots на том же диске недостаточны.

## Практический пример Lobachevsky — Docker GC

В KS54 удаление только dangling images не охватывало tagged releases. Перед изменением проверили systemd timer, deploy flock и Docker images. Dry-run отобрал 57 кандидатов; при контролируемом apply 54 удалены, 3 Docker отклонил из-за нескольких tags (без force). BuildKit cache pruned отдельно; свободный диск вырос примерно с **97 GiB до 165 GiB**, критические контейнеры были healthy. Текущие данные — историческая диагностика, не вечная метрика. Эксплуатационный урок: защищать running/stopped container IDs, current/previous rollback, shared layers, volumes, production DB; не использовать слепой system prune with volumes. Различать EXECUTED, SKIPPED_LOCK и WARN; сохранять журнал GC. Не копировать KS54 скрипт на E7A без оценки других проектов.

## Self-hosted CI

Временные worktree/контейнеры для PR, immutable head SHA, CPU/RAM/disk лимиты, isolation Docker socket, полные логи, подтверждение результатов и cleanup после merge. Наличие чернового прототипа KS54 PR #761 — не production готовность E7A runner.

## Никакой несанкционированной автоматики

По умолчанию никакой git push, создание репозитория или неразрешённый merge не разворачивает ВМ/DNS/базы. **Исключение для выпуска кода продукта:** после явной однократной активации и в границах standing authorization merge в защищённый `main` запускает автоматический self-hosted QA/build/deploy без пострелизного owner-click. Infra применяет только разрешённый service-scoped план; GitHub-hosted Actions CI запрещён.
