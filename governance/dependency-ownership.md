# E7A — единый стандарт владения зависимостями и их обновления

**Решение Owner E7A от 06.11.2026: УТВЕРЖДЕНО как обратно совместимое расширение Governance 2.1.0 (MINOR).** [Нормативный release descriptor](standards-release.json) определяет **действующую версию и точный immutable SHA**. Пока descriptor указывает `2.0.0` / `298fe2856f6ac879286f96d089175fa960c88dd8`, это правило ещё не действует как релиз 2.1.0. После отдельного утверждённого descriptor `2.1.0` оно является частью нового нормативного baseline. Само слияние исходного PR не заменяет выпуск descriptor, не активирует ботов и не меняет consumer pins. Для существующих продуктов обязательство внедряется через отдельные review PR без массового автоматического обновления.

**Применимость после утверждения:** все E7A продуктовые и инженерные репозитории, включая `core`, `website`, `reader`, `bot` (Studio и API/Worker отдельно), `gateway`, `planner`, `infra`, `laboratory`, `automation`, `identity`, `platform`, а также новые продукты. Kit определяет применимые проверки, но не отменяет границы владения.

## 1. Единственный источник нормативной политики, разные владельцы исполняемого кода

| Компонент / ответственность | Владеет | Не получает автоматически |
| --- | --- | --- |
| `.github/governance` / Founder-Owner E7A | Общие требования, роли, исключения и утверждённую версию политики | Состояние runtime/QA конкретного продукта |
| **Core** | Общие независимо версионируемые `@e7a-ru/ui` (React/CSS, брендовые контракты), private `@e7a-ru/server` (нейтральные backend-контракты), их immutable release/evidence; **необязательный** Product Starter | Чужие lockfile, production framework, БД, продуктовые сервисы, auth и deploy |
| **Владелец каждого продукта** | Собственный `package.json` и lockfile, Next.js/React/Node и другие runtime dependencies, domain/API/data, compatibility, QA, security patches, rollout и rollback | Право выпускать новый Core пакет, менять соседние продуктовые `main` или игнорировать Governance |
| **Automation** | Read-only manifest/registry/governance inventory, impact и подготовку reviewable dependency/Governance предложения/отчёта | Прямой push в `main`, package install/edit, выдачу write-токенов, массовое обновление, auto-merge и deployment |
| **Infra / Laboratory** | Infra: разрешённые host/secrets/deployment/backup/rollback; Laboratory: релевантную подготовку QA/evidence, когда внедрена | Подмену владельца исходного продукта или доказательство QA без запуска |

**Нет общего централизованного `package.json` для E7A.** Core Starter — начальный *копируемый* пример: обновление его зависимости отражается на **будущих** генерациях, но никогда не обновляет существующие Website, Reader, Bot или Gateway. `@e7a-ru/ui` не тянет Next.js; `@e7a-ru/server` не становится общим HTTP API/Identity/DB/MQ.

## 2. Критерии допустимости общего кода в Core

Предложение в Core требует: независимости от конкретного домена, публичного стабильного API, контрактных и негативных тестов, versioned changelog и доказанной пользы как минимум двух продуктовых контекстов **или** статуса фундаментального брендинга/безопасности. Для нового пакета или breaking API — ADR, migration и owner review. Не переносить в Core маршруты, бизнес-правила, продуктовые Next pages, session/auth, данные/миграции, Pg/RabbitMQ, privileged Gateway helpers, ключи и deploy controllers. Эти границы согласуются с [Product Stewardship](product-stewardship.md) и [трёмя Kit](kit-implementation-roadmaps.md).

## 3. Exact runtime pins, источники и воспроизводимость

1. Для **прямых runtime dependencies** продуктовые `package.json`/manifest фиксируют **точную опубликованную SemVer-версию** без `^`, `~`, `latest`, `workspace:*`, mutable Git branches и незакреплённых source tarballs. Пример: `"next": "16.4.5"`, `"@e7a-ru/ui": "0.3.0"`; пример **не** означает, что любой продукт обязан использовать именно эти версии или уже их внедрил.
2. Свой lockfile (`pnpm-lock.yaml`, `package-lock.json` и т. п.) создаётся настоящим package manager, включается в продуктовый PR и проверяется через **frozen install на exact PR SHA**. Не изменять integrity или версии lockfile вручную; transitive dependencies фиксирует lockfile. Product managers могут разрешить scoped исключения для devDependencies при контролируемой воспроизводимости; **library peerDependencies** закономерно задают диапазон совместимости и не являются product runtime pin.
3. **Core UI** ставится только из реально подтверждённого public npm (`registry.npmjs.org`), а private Server — только из отдельно подтверждённого GitHub Packages (`npm.pkg.github.com`) в **backend install context** с минимальными read-permissions. Не добавлять в frontend глобальный scope mapping `@e7a-ru → npm.pkg.github.com`, иначе публичный UI перестанет резолвиться как public. Никаких токенов, OTP или секретов в Git/логах/PR.
4. `@e7a-ru/ui@0.3.0` — исторически подтверждённый immutable public npm релиз; **не републиковать**. `@e7a-ru/server@0.1.0` — ранее опубликованный private релиз; source-кандидат `server@0.2.0` **не считается опубликованным**, пока нет отдельного owner-authorized publish и authenticated readback. Релиз определяется registry evidence, не только `package.json`, tag или прохождением QA.
5. Каждое новое Security Advisory обрабатывается в **обоих независимых местах**: Core обновляет уязвимые общие библиотеки и default будущего Starter, а владельцы уже созданных продуктов исправляют собственные точные framework/runtime pins. Не ждать общего Core релиза для самостоятельного исправления Next.js в Gateway; не считать обновлённый Starter признаком обновления уже запущенного продукта.

## 4. Ручной dependency update PR и его доказательства

Каждое изменение затронутого продукта: старая/новая точная версия и реестр, scope, основание/security advisory, изменённый lockfile, API/peer/branding/license impact, собственный rollback, PR-owner/reviewer. **Ни одна registry readback, Core npm publication, Governance pin или Automation report не равна product adoption.**

Проверки в изолированной локальной/self-hosted среде на **точном SHA**: frozen install; typecheck/lint; unit/negative; production build; dependency/security audit; browser/a11y/visual для UI; auth/integration/migrations/backup для backend по Kit и риску. Сохранить host, SHA, lock digest, команды и PASS/FAIL/UNKNOWN. Релиз/продакшен выполняются по [Release & Deployment policy](release-deployment-policy.md), после отдельного product activation; staging — по реальному риску. **GitHub-hosted Actions CI запрещён**; исключение — только отдельно разрешённая ручная публикация Core-пакета.

Критические advisories требуют своевременного triage: подтверждённые затронутые версии/функции, срочность, mitigation, владелец и решение. Если исправление ещё не принято, статус `BLOCKED`/`NOT_VERIFIED`, а не утверждение «уязвимостей нет». Наличие green npm audit относится только ко времени конкретного запуска.

## 5. Automation / Governance / consumer propagation

**Automation** может на разрешённом read-only scope сверять immutable Core/Governance SHAs, публичный npm metadata, private registry status при отдельно разрешённом read, product manifests и **наличие** lockfile, готовить `POTENTIAL_COMPATIBILITY_REVIEW` или offline handoff. Это **не** attest lockfile integrity, не продуктовый QA и не разрешение править репозитории.

Ни один организационный normative change не применяется массово вследствие merge документа: Owner утверждает ADR/impact, публикует следующую **immutable version** descriptor, затем для каждого затронутого consumer отдельный review PR с `governance-consumer.json` pin на утверждённый normative SHA. См. [Standards propagation](standards-propagation.md).

Future PR-writer с `Contents: write`, создание GitHub App/таймера и product auto-upgrade — отдельные полномочия, risk review и canary; текущий план не активирует их. Automation не заменяет нативный Dependabot: после первоначального закрепления продукта Dependabot может предложить совместимые patch PR без auto-merge или GitHub Actions CI; второй агент не должен создавать дубль на ту же задачу.

**Строго разделять:** `PROPOSED` → `SOURCE_MERGED` → `TESTED_EXACT_SHA` → `PUBLISHED` → `PRODUCT_ADOPTED` → `DEPLOYED/RUNNING_VERIFIED`. Отсутствие проверок даёт `NOT_VERIFIED`, `UNKNOWN`, `SKIPPED` или `BLOCKED`, но не `PASS`. Для baseline/version берётся [standards-release.json](standards-release.json), а не mutable `main` и не этот ещё не утверждённый текст.

## 6. Политика бренда, лицензий и исключений

Public `@e7a-ru/ui` сейчас декларирует `UNLICENSED`: скачивание не предоставляет третьим сторонам общую лицензию на код, E7A Brand Assets и товарные знаки; действует отдельная [Brand Policy](../BRAND_POLICY.md). Изменение лицензии — отдельное решение правообладателей. Любое исключение из runtime exact pins/Governance правил должно иметь owner-approved ADR, срок действия, компенсирующие меры и соответствующий release scope; local README не отменяет общеорганизационную норму.

## 7. Порядок принятия этой нормы

**Решение о содержании утверждено владельцем 06.11.2026; вступление в силу определяется отдельной версией `standards-release.json`.** Последовательность: owner-approved исходный Governance PR → зафиксированный merge SHA нормативного дерева → отдельный reviewed `standards-release.json` `2.1.0` с этим immutable SHA, impact/migration/Owner-approval → только после этого targeted consumer adoption PR. Existing approved Governance 2.0.0 действует до отдельного выпуска 2.1.0. Новым продуктам применять утверждённое правило сразу после релиза; существующим — при ближайшем профильном PR или security-обновлении, с фиксацией временного дрейфа и без принудительного mass update. Примерные версии пакетов в документе не являются universal dependency lock.

Связанные правила: [Standards & Change Control](standards-and-change-control.md) · [Product Stewardship](product-stewardship.md) · [Release/Deployment](release-deployment-policy.md) · [E7A Automation](engineering-automation.md) · [Core implementation policy](https://github.com/E7A-RU/core/blob/main/docs/governance/core-update-policy.md).
