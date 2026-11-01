# E7A — упрощённый release/deployment стандарт для продуктовой линейки

**Статус: OWNER-DIRECTED POLICY / NORMATIVE ONLY AFTER VERSIONED GOVERNANCE RELEASE.** Направление подтверждено владельцем 2026-11-01; применимую immutable ревизию определяет [опубликованный Governance descriptor](standards-release.json). Даже после публикации стандарта этот документ **не активирует** timer, не меняет production, права GitHub App или секреты. Каждому продукту требуется отдельное подтверждение готовности и включение.

## Решение и базовый принцип

**Продукт должен выпускаться быстро, воспроизводимо и без лишней ручной работы.** Для одобренных классов изменений стандартный путь:

```text
PR → review/merge в защищённый main
   → self-hosted detection (poller/systemd timer)
   → exact-SHA QA / security checks (FAIL = STOP)
   → immutable build + digest
   → product-specific preflight/rollout safety
   → production deployment
   → health/smoke verification → SUCCESS | rollback + incident
```

- **Нет обязательного staging** для обычного выпуска. Изолированная QA-сборка и disposable environment допустимы, но не называются staging. Staging/canary включаются **по реальной необходимости** — если риски, сложность интеграции или контракты конкретного продукта требуют отдельной проверки.
- **Нет обязательного ручного подтверждения после сборки** для разрешённых низкорисковых изменений. Слияние проверенного PR в защищённую `main` является командой на выпуск **только** там, где продукту заранее выдано ограниченное постоянное разрешение (standing authorization) и готова самохостинговая release-конфигурация.
- **Единой кнопки «деплоить всё» нет.** У каждого продукта свой lockfile, Kit, набор QA, rollout, история версий, health и rollback; Core package release не перезапускает consumers. Не применяем GitHub-hosted Actions для CI. Отдельно разрешённая ручная публикация пакетов Core — другое действие.
- **Без сторонних обязательных платформ.** Первый практический вариант: существующий Lobachevsky, отдельный Docker Compose project, self-hosted `systemd timer`, ограниченный polling, журнал, блокировка параллельных запусков и сохранённый предыдущий образ. Не требуется сначала построить Laboratory, централизованный Release Controller, дополнительный сервер или staging.

## Где и как выдаётся право на auto-deploy

Перед **первым включением** для каждого продукта Owner утверждает конфигурацию standing authorization, содержащую: product/repository, целевую защищённую ветку, разрешённые R0/R1 классы и точные исключения, target environment/host/service, runtime identity, обязательные тесты/thresholds, budget/stop policy, правила expiry/revocation, audit и путь восстановления. Infra выполняет проверенный dry-run и фиксирует факт отдельного включения. **Наличие этого документа или merge policy PR не активирует автодеплой.**

После активации не требуется заново спрашивать Owner о каждом **обычном** релизе, если:

1. Изменение принято в защищённую `main` уполномоченным ревью/merge; не существует uncontrolled direct-push/auto-merge обхода.
2. **Проверенный фактический diff** относительно последнего deployed SHA входит в конкретный allowlist standing authorization. Полагаться только на PR label/branch name/текст commit запрещено. Любые `UNKNOWN`, `SKIPPED`, неразрешённые изменения или отсутствие обязательных доказательств блокируют выпуск; непройденные необязательные gates оформляются как `NOT_REQUIRED` с причиной.
3. QA проверяет **точный полный commit SHA** в изоляции от production-секретов/привилегированного Docker socket. Frozen dependencies, type/lint/unit/integration/security/negative/browser проверки — по выбранному Kit.
4. В production поступает **тот же проверенный immutable artifact digest**, без непроверенной пересборки. Проверены supply chain, конфигурация, host blast radius, доступность rollback.
5. Переключение ограничено **только этим продуктом**, есть lock от гонок и отдельный процесс QA без доступа к deployment credentials, безопасные таймауты, журнал, post-deploy healthcheck и автоматическая попытка rollback. Невосстанавливаемый сбой означает **STOP + alert/ручной инцидент**, а не фиктивный PASS.

## Когда требуется отдельное решение владельца

Standing authorization **не охватывает** по умолчанию R2+ и изменения с риском даже при ошибочной метке R0/R1: инфраструктурные/сетевые настройки и маршруты, WireGuard/Gateway, IAM/Identity, ключи и секреты, privileged helper, платёжные/биллинговые потоки, tenant boundaries, потерю/перенос пользовательских данных, несовместимые DB миграции, release/deploy controller и его права, изменения политики Governance и исключения security/privacy. Для них нужен отдельный review, explicit release approval и адресные доказательства (включая staging, восстановление, канареечный rollout, dual-control — **когда оправдано риском**).

Если нет безопасного rollback, необходима forward-recovery/backup/migration стратегия **до** выпуска. Простое сохранение предыдущего контейнера **не откатывает базу данных**.

## Кто отвечает

| Слой | Обязанность |
| --- | --- |
| Product / инженерный агент | Продуктовый код, PR/review, тесты, сборка, smoke/health, классификация риска, совместимость и продуктовый rollback |
| Infra | Ограниченный self-hosted executor, host/isolation, артефакт, credentials, service switch, журнал, disaster recovery и emergency stop |
| Laboratory (по мере появления) | Дополнительная организация QA/evidence, но не обязательный runtime dependency каждого релиза |
| E7A Automation | Governance/зависимости: **предложения Draft PR**, не привилегированный deployment controller |
| Owner E7A | Однократно утверждает рамки и включение standing authorization; отдельно утверждает изменения вне рамок |

## Принятие на продукте

1. Снять live-readback текущих main/production SHA, cron/systemd, контейнера, портов, секретов и механизмов восстановления. **Старый reset-first cron не включать.**
2. В отдельном PR подготовить локальные команды проверок, allowlist/denylist, dry-run, безопасный fail-injection, manifest с source SHA/digest, журнал и recovery.
3. На точном SHA доказать: QA PASS → build → безопасный rollout; QA FAIL/неизвестный результат → production не меняется; health FAIL → rollback или эскалация.
4. Получить разовое разрешение Owner на **включение конкретного продукта**, подтвердить запущенный timer и первый реальный успешный deployment. Только после этого обозначить `AUTO_DEPLOY_ACTIVE`. До этого `PROPOSED`/`IMPLEMENTED_NOT_ACTIVE`.
5. Другие продукты повторяют процесс независимо. Начать с **Website на Lobachevsky**; Reader, Bot и прочие выбирают свои gates. Нельзя расширять Website-разрешение на все продукты автоматически.

## Запись релиза и статус

На каждый выпуск сохраняются: продукт/target, exact source SHA, artifact digest, risk decision/standing authorization ID **или** individual approval ID, QA timestamp/logs, current/previous artifact, deployment outcome, healthcheck/rollback evidence. Отсутствие записи означает `UNKNOWN`, а не успешный релиз.

Пример статусов: `SOURCE_MERGED → QA_PASS → BUILT → DEPLOYED → RUNNING_VERIFIED`, либо `QA_FAIL / BLOCKED / ROLLED_BACK / RECOVERY_REQUIRED`. **Staging не является обязательным промежуточным статусом.** При risk-triggered staging указываем `STAGING_VERIFIED` с отдельным доказательством.

## Управление версией нормы

Это изменение заменяет прежний безусловный путь «staging → отдельное одобрение каждого production-релиза» **только с момента** принятия Governance PR и публикации новой immutable версии стандарта. До этого для runtime действует предыдущая утверждённая политика. Consumer governance pins обновляются только через targeted review; сам факт новой версии Governance **не запускает production deployment**.
