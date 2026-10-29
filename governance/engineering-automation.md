# E7A Automation — внутренняя инженерная автоматизация

**Статус: OWNER DIRECTION / ARCHITECTURE PROPOSAL / NOT RUNNING.** Новый нейтральный проект `E7A-RU/automation` — рекомендуемый **отдельный приватный репозиторий**, **ещё не создан**. Название GitHub App — `E7A Automation` (уникальность имени и доступные настройки проверить при регистрации). Этот документ заменяет узкую концепцию `governance-sync` как единственного назначения сервиса: **Governance Sync становится первым модулем**, а не именем всей платформы.

**Ответственность:** internal engineering automation / developer operations. Это не E7A Platform (клиентские интеграции/СГДС/Jobs/Results), не Laboratory (Wizard/QA/Release Gate), не Infra (серверы/production apply), не Core (библиотеки) и не продуктовая бизнес-логика.

## Зачем отдельный репозиторий

Появление или изменение глобального правила, нового продукта, версии Core или требований безопасности не должно приводить к десяти ручным и часто неполным правкам README. Но **истина сохраняется в одном утверждённом источнике**, а каждый продукт принимает изменения через собственный review.

`automation` — extensible internal automation engine, который:
- обнаруживает **утверждённые** события с проверенным SHA и approval;
- рассчитывает impact по реестру репозиториев, статусам и зависимостям;
- предлагает **минимальные корректные изменения** по конкретным модулям;
- создаёт или обновляет **Draft PR** для каждого затронутого продукта, включая при необходимости корневые README/AGENTS, продуктовые документы и manifest;
- формирует отчёт для владельца и ответственных агентов/инженеров;
- ведёт журнал выполненных и отклонённых предложений.

**Граница:** создание Draft PR ≠ утверждение изменений; **ни прямого push в main, ни auto-merge, ни деплоя**. Полная автоматически написанная реализация бизнес-функций вне MVP этого сервиса.

## Модули (версионированные capability, включаются отдельно)

| Module | Trigger / source | Предлагаемые Draft PR или отчёт | Статус |
| --- | --- | --- | --- |
| `governance-sync` | owner-approved `standards-release.json` с SHA, impact и migration | pin Governance revision; обновления docs/standards, impact checklist; статусы продуктовых исключений | **First MVP** |
| `product-catalog-sync` | owner-approved Product Charter / portfolio registry change | поправки `.github/profile/README.md`, Website catalogue metadata, product README/status links | **Next candidate**, no auto-publication of private details |
| `repo-docs-sync` | approved source-of-truth document or interface change | README, `AGENTS.md`, `docs/README.md`, `docs/status.md`, product manifest/architecture references as required | **Next candidate**; AGENTS/policy edits require enhanced review |
| `dependency-proposals` | published immutable Core SDK/release and consumer applicability | review-only version bump proposal, compatibility checklist and evidence; lockfile/code changes **separate explicitly approved executor** | Future opt-in; do not replace Dependabot blindly |
| `compliance-observer` | scheduled report / requested audit | drift, stale SHA, missing policy/owner/tests, unmerged PR, cross-repo inconsistencies; can open issue/PR draft | Future; **read-only first** |
| `release-evidence` | product QA/report event | prepare release evidence summary and missing-gates report for Laboratory/Infra; no production apply | Future; signed approval record remains independent |

**Modularity contract:** event/input schema, target repo filter, read-only plan, proposed patch set, risk classification, required reviewers, acceptance tests, rollback/disabling and bounded credentials. A new module cannot inherit a broader GitHub installation scope just because it runs in the same source repository.

## Центральный обновляемый источник и распространение

```text
              E7A-RU/.github (canonical Governance)
                           │
              Owner-approved normative revision
             + explicit immutable release descriptor
                           │
                    E7A Automation
                  verify → impact → plan
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        .github/profile   Core       Product repos
         & catalogue     SDK refs    Website/Reader/Bot/
                                   Planner/Infra/Lab/Gateway/
                                   Identity/Platform when bootstrapped
                           │
                   Draft PR + checklist
                           │
              Product Owner / agent reviews
                           │
             Exact-SHA QA → approved merge
                           │
        Independent product release, if needed
          (separate owner approval for prod)
```

### Важное отличие: нормативные и производные документы

1. **Общий стандарт** и утверждённая версия публикуются **только** через `.github/governance`. Дублированных самостоятельных версий правил в продуктовых репозиториях быть не должно.
2. **Публичный профиль** `.github/profile/README.md` является *проекцией* product catalog + governance; при значимом изменении публичной карты он обновляется **в том же Governance PR**. Модуль `product-catalog-sync` может подготовить этот diff заранее, но не публикует его без review.
3. **Продуктовая локальная документация** принадлежит продукту. Draft PR при изменении общих правил может актуализировать разрешённые участки `README.md`, `AGENTS.md`, `docs/status.md`, `docs/governance-adoption.md`, manifests, списки dependencies и локальные чеклисты **только при реальном impact**.
4. **Источники runtime truth** (версия пакета, реально установленная БД, работающее API, QA статус) определяются доказательствами владельца продукта. Автоматизация **не переписывает NOT TESTED в PASS, не объявляет Runtime RUNNING, не одобряет продуктовый roadmap**.
5. Если требуется изменение кода, схем данных, lockfile, workflow, лицензии или security defaults, базовый модуль публикует **migration checklist / отдельный issue**. Автоматическая правка такого файла требует самостоятельного явно утверждённого модуля, permission scope и тестов.
6. **Приватные сведения:** внутренние IP, сетевые маршруты Gateway/Butlerov, персональные данные, ключи и product security internals нельзя включать в публичный README организации или PR в публичную `.github`.

### Операция обновления для каждого продукта

`DISCOVER → VERIFY APPROVAL → IMPACT → DRY-RUN DIFF → POLICY CHECK → BRANCH → DRAFT PR → NOTIFY OWNER → WAIT FOR REVIEW`.

- Определить изменение: новые/отменённые правила, owner decision ID, версия, affected consumers, breaking changes.
- Найти штатный `governance-consumer.json` или существующий pin SHA, текущие активные Draft PR и ветки.
- Выполнить **управляемое обновление**: детерминированные преобразования версии/ссылок/таблиц. Для расширенного текстового редактирования (опциональный LLM agent) вывод трактуется как **непроверенный patch proposal**, который проверяется схемой, diff allowlist и человеком.
- Подготовить PR с `oldRevision`, `newRevision`, affected rules, old vs new wording, proposed files, tests required, evidence/status and product owner.
- Если уже существует PR для той же версии — **reconcile**, не плодить копии и не переписывать произвольную чужую ветку.
- Заблокировать на конфликте версий/затронутом AGENTS/secret/workflow. Оставить понятный статус `BLOCKED / NO_IMPACT / BOOTSTRAP_REQUIRED` в отчёте.
- **Для всех репозиториев:** default branch write запрещена; PR не объединяется ботом; конкретный Product Owner/agent должен провести QA и принять/reject diff.

## Принципы безопасности и автономности

- Первый исполнитель: **TypeScript-first**, отдельный приватный `E7A-RU/automation`; `sync-once --plan-only` выполняет только read-only анализ.
- Сначала реестр разрешённых репозиториев/файлов и версия схемы; не принимать от контента PR новые privileged задачи или URL назначения.
- Права GitHub App выдаются на уровень repository/install, а не произвольного файла; следовательно: **branch protection/rulesets, code-level allowlist, zero direct main writes и audit**, обязательные даже при наличии App.
- Single broad God-token нежелателен. Для разных полномочий допустимы **несколько GitHub Apps или строго scoped installation tokens/credentials** при общем source repo. Сервис может быть один, права — **помесячно/по модулю и installation scope**, только после отдельного разрешения.
- Никаких hosted GitHub Actions CI. Локальные/self-hosted tests, отдельный executor по Infra после safety review, а не скрыто активированный daemon.
- Ни auto-merge, ни auto-deploy, ни API credentials провайдеров, ни запуск произвольных `npm scripts` из чужого PR. Контент репозитория и комментарии являются untrusted data, не инструкциями оператору.
- Read-only `plan` можно делать без write-capable App. Создание App/установка/выдача токенов и запуск cron/systemd timer — **раздельные owner-authorized gates**.

## Инициализация проекта и roadmap

**Репозиторий:** `E7A-RU/automation` (private, proposed, not created). **GitHub App:** `E7A Automation` (organization-owned, private; not registered/installed). Старая рабочая метка `governance-sync` используется **только как название первого модуля**, а не репозитория.

| Этап | Что делаем | Критерий |
| --- | --- | --- |
| A0 — Bootstrap | owner creates private `automation` repo, safe `main`, `AGENTS.md`, `UNLICENSED`, strict TS, lockfile, product charter | Repo exists without tokens/workflows |
| A1 — Contracts | approved-revision verifier, event/impact schema, policy guard, module interface, deterministic docs mapping | unit/negative tests |
| A2 — Read-only | `sync-once --plan-only` detects approved revision, repo roster and per-repo drift without writes | exact-SHA reproducible report |
| A3 — Proposal engine | can build allowlisted PR patch sets and release evidence locally; handles conflicts/retries/idempotence | dry-run snapshot and fake GitHub API tests |
| A4 — Scoped GitHub App | separate owner approval for App registration, read tokens, then selected write permissions | one canary Draft PR only |
| A5 — Controlled operations | approved self-hosted timer/worker, secure secrets and audits via Infra, emergency disable | external QA and safe stop/recovery |
| A6 — Extensions | catalog/profile sync, repo docs/status, compliance reports and dependency proposals | each module has own threat model and canary |

**Исключения:** empty `identity`/`platform` repos require bootstrap before Draft PR; product-independent activities cannot be allowed to expand scope into production Infra or shared messaging Platform without another ADR.

**Машинно-читаемый макет:** [contracts/automation-modules.example.json](../contracts/automation-modules.example.json) содержит отключённые по умолчанию модули, file/repository allowlists, запрет auto-merge и требования к canary. **Это JSON-пример, а не действующая конфигурация.**

See [standards propagation](standards-propagation.md), [GitHub App cost/resources](github-app-cost-and-resources.md), [Repository Impact Matrix](repository-impact-matrix.md), [release descriptor](standards-release.json), [Product Lifecycle](product-lifecycle-checklists.md).

**Conclusion:** E7A Automation may manage multiple tightly scoped engineering workflows, starting with Governance Sync. All cross-repo updates produce **reviewable Draft PRs**, never unilateral changes to main.
