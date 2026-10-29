# E7A Governance Sync GitHub App — стоимость, ресурсы и ограничения

**Статус:** оценка инженерного решения; [отдельный планируемый приватный `E7A-RU/governance-sync`](governance-sync-service.md) и **не созданное/не установленное GitHub App**; расчёт и документация от 2026-10-30. Связанный [Standards Propagation](standards-propagation.md) остаётся концепцией; owner approval и отдельный Infra PR обязательны перед запуском.

## Что оплачивается

- **Собственная GitHub App** (не Marketplace-подписка на сторонний сервис) — API-механизм интеграции. Регистрация собственной App сама по себе не означает покупку тарифа/платное место участника. GitHub подтверждает, что GitHub Apps **не расходуют user seat**. GitHub App может быть private, доступной только организации-владельцу.
- **Marketplace сторонних Apps** может иметь отдельные free/paid планы — **не то, что нужно для E7A Sync**. Не требуется публикация собственной App в Marketplace.
- **GitHub API** имеет rate limits, а не тариф за каждый стандартный REST запрос. Для installation access token обычно базовый лимит **5 000 запросов/час**, масштабируется с количеством пользователей/репозиториев, Enterprise Cloud иным образом. 11 проектов E7A и несколько проверок в день существенно ниже обычных API лимитов при нормальном backoff.
- **Собственные расходы:** хостинг исполняющего процесса (VM/container), резервирование и журналирование, дисковое пространство под state/logs, мониторинг, исходящая сеть/домен/TLS **только если нужен webhook receiver**, поддержка безопасности и ротации ключей.
- **GitHub Actions:** использование **не требуется**. Для E7A-hosted CI действует запрет; запуск App сам по себе не создаёт workflow и не тратит Actions minutes.

## Ресурсы для первого прототипа

| Вариант | Ресурсы (предварительная инженерная оценка, не проведённый benchmark) | Особенности |
| --- | --- | --- |
| **A. systemd timer + короткоживущий poller** (рекомендуем) | Начальный лимит **0.25 vCPU / 256–512 MiB RAM**, десятки MiB кода/состояния + ограниченные логи; ежечасная/дневная проверка | Нет публично доступного webhook и постоянного web process; запуск в отдельной изолированной среде, а не на production DB/worker |
| B. Постоянный webhook receiver | Начальный лимит **0.5 vCPU / 512 MiB RAM** плюс TLS/ingress/replay queue | Больше эксплуатационных/сетевых рисков; webhook signature verification и retry обязательны |
| C. БД-ориентированный управляющий worker | A или B + **выделенная PostgreSQL DB/role** под durable reconciliation/leases | Обоснован после multi-instance/high-volume роста; не требовать новый PostgreSQL исключительно ради простого read-only poller |

Это **бюджетные гипотезы** для маленького объёма E7A, не доказанные требования и не заявленные свободные ресурсы Lobachevsky. Реальный host placement, CPU/RAM/disk quotas, blast radius и off-host backup утверждает Infra по свежей инвентаризации. Можно начать с `plan-only` на существующей безопасной QA VM **если она реально выделена**.

## Доступ к GitHub

- Одна **private organization-owned GitHub App**, установка только на нужные репозитории.
- App permissions: `Metadata: read`, `Contents: read` для `.github`, `Contents: write` и `Pull requests: write` только в выбранных product repos. GitHub app repository-level permissions действуют в рамках installation scope; различать чтение и запись внутри одного repo может потребовать **две App установки/учётные роли** или сервисный enforce allowlist — это security ADR, а не гарантия API уровня.
- У приложения нет `administration`, `actions:write`, `deployments:write`, `secrets`, `workflows`, `merge` или других ненужных полномочий. **Важное уточнение:** право `Pull requests: write` позволяет управлять PR, а контролировать недопустимый merge следует защитой веток и кодом/policy — не утверждать, будто GitHub имеет отдельный `no-merge` permission.
- Короткоживущие installation tokens действуют **около 1 часа**; создавать при запуске, не хранить длительно. Секрет App private key хранить в Infra secret store с ротацией.
- Опционально для исходного режима `plan-only` — вообще не выдавать write access; повышать права только по отдельному разрешению владельца после теста.
- Логи только metadata: repo, source SHA, target SHA, rule diff IDs, PR id, status; никаких токенов, personal data, network details или content dumps.

## Оценка для E7A

**На GitHub — не ожидается обязательной отдельной платы за собственную организационную App при обычном API использовании.** Однако нельзя гарантировать нулевую итоговую стоимость всей автоматизации: собственный сервер и обслуживание стоят ресурсов; Marketplace paid integrations не входят в этот расчёт. Текущие цены стороннего хостинга не оценивались, так как место исполнения ещё не выбрано.

**Рекомендуемый порядок:** docs/adoption report → `plan-only` без GitHub write token → один Draft PR на неважный repo → аудит/восстановление/отключение → разрешённый self-hosted timer → масштабирование. Полностью отключённое auto-merge и production apply сохраняется.

## Подтверждения в документации GitHub

- [Customizing organization's profile](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile)
- [About creating GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/about-creating-github-apps)
- [Deciding when to build a GitHub App](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/deciding-when-to-build-a-github-app)
- [GitHub App installation rate limits](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api)
- [Installation access token validity](https://docs.github.com/en/rest/apps/apps)
- [Private GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/making-a-github-app-public-or-private)
- [Marketplace pricing plans](https://docs.github.com/en/apps/github-marketplace/listing-an-app-on-github-marketplace/setting-pricing-plans-for-your-listing)

**This is a product architecture recommendation**, not an assertion that any App has already been created, authorized, billed, or installed.
