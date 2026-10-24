# Platform RFC: backend capabilities, СГДС, Jobs, Connectors, Results

**Статус: согласована целевая область, не выбрана технология и не создан рабочий E7A Platform.** Не смешивать интеграционные намерения с уже запущенным Bot Studio.

## Почему не просто «превратить Bot Studio в интеграционный монолит»

Bot Studio уже специализируется на разговорах, сценариях диалогов, bot adapters, проектном доступе, webhook, PostgreSQL inbox/outbox и RabbitMQ. Расширение до arbitrary SaaS actions, email delivery, файлов, SMM publishing и всех API изменило бы продуктовые границы и увеличило blast radius. **Bot остаётся специализированным продуктом**, а общие capability и адаптерные контракты могут быть переиспользованы после независимого ADR и тестов. Не копировать секреты, табличные схемы или пользовательские данные Bot в Platform.

## Четыре логических контура Platform

| Контур | Отвечает за | Не отвечает за |
| --- | --- | --- |
| **Delivery / СГДС** | Durable принятие commands/events, маршрутизация envelopes, повторные попытки, dead-letter, квитанции, subscriber notifications | Абсолютное выполнение внешнего бизнес-действия |
| **Jobs / Execution** | Типизированные job types, leases, bounded workers, idempotency, retries/timeout/cancel, result state | Универсальное исполнение произвольного недоверенного кода |
| **Connectors / Integration** | Управление поддерживаемыми provider capabilities, scoped подключениями, OAuth/tokens, ограничения API, media conversion, egress policy | Выдача всем сервисам чужих credentials, обход правил provider |
| **Results / Operations Registry** | operation/batch ID, агрегирование частичных исходов, provider IDs, audit, статус API, signed result events/webhooks | Смешивание provider-acceptance с фактом прочтения адресатом |

В v1 все четыре могут быть модулями **одного тщательно разделённого модульного runtime**, не обязательно четырьмя сервисами. Разделять технически только по подтверждённым независимым SLA/нагрузке/безопасности.

## Гарантия СГДС — строго по уровням

**Accepted:** durable commit до ответа отправителю, с operationId и дедупликацией. Если WAL и backups на одном физическом сервере — потеря хоста всё равно может привести к потере данных; гарантия ограничена явным durability/SLO.

**Dispatched:** задача передана исполнителю, не эквивалент delivery.

**Delivered:** именно тот вид подтверждения, который умеет доказать канал: провайдер принял API/SMTP hop, подписчик сохранил event, webhook вернул валидный ACK. API 200 не всегда значит публикацию, а SMTP 250 не гарантирует inbox.

**Processed:** потребитель прислал подтверждение результата в согласованном контракте. Не означает объективно правильное действие, если внешний мир не подтверждает. Разделять SUCCEEDED, FAILED, UNKNOWN (после timeout неопределённый исход), RETRYING, DEAD_LETTER, EXPIRED, CANCELLED. Для внешнего провайдера не дублировать потенциально выполненное действие без подтверждённой идемпотентности.

Стандарт: at-least-once, idempotency keys per producer+recipient+operation, transactional inbox/outbox, persistent state before ACK, bounded retries/backoff/jitter, rate limits, partitions, audit, dead-letter, poison-message controls. **Exactly-once внешнего side effect не обещаем.**

## Как будет исполнен комплексный запрос

**Иллюстрация, не выполненная операция:** инициатор в Planner: «Пете через MAX отправить файл, Васе через Telegram текст, в Instagram опубликовать статью».

1. Planner с проверенной Identity создаёт batch (3 операции) и передаёт в Platform; Platform проверяет аудиторию, tenant, доступ к конкретным подключённым аккаунтам и rate/consent.
2. Delivery durable записывает batchId и 3 operationId, отвечает **Accepted** (не «отправлено»).
3. Jobs dispatcher выбирает connector capability и ставит задачи в разрешённые worker pools.
4. MAX connector передаёт разрешённый файл (media/size/safety/expiry), Telegram connector отправляет текст, Instagram connector проверяет ограничения официального API/типа аккаунта/формата поста/разрешений — действие может быть запрещено или требовать подтверждения.
5. Каждый connector фиксирует provider receipt или неопределённый исход, обновляет Result Registry. Fail одной операции не должен скрыть success остальных.
6. СГДС отправляет подписанные result webhooks или события **каждому разрешённому инициатору/подписчику**, а polling GET status позволяет восстановить состояние после потери callback. Авторизация результата по tenant, audience и data minimization.
7. Итог batch = ALL_SUCCEEDED, PARTIALLY_SUCCEEDED, FAILED, PENDING, UNKNOWN; корреляция по batchId/operationId и timestamps.

**Запрос нескольких инициаторов:** не рассылать всем все чужие детали. Хранить subscribers и scopes, рассылать только разрешённый snapshot; webhook signing, retries, SSRF/egress allowlist и безопасное storage для файла.

Никакая абстрактная модель не доказывает, что конкретный аккаунт Telegram/MAX/Instagram поддерживает эти API-возможности. Указывать per-provider capability matrix и актуальные разрешения. СГДС не должна самостоятельно генерировать статью/картинки или обходить правила платформ.

## Connector SDK

Capability manifest: connector ID/version, operations (например message.send, file.send, post.publish), auth mode, secret scope, accepted MIME/size, rate/window limits, idempotency available?, reliable provider receipt?, webhook signature/protocol, allowed egress destinations, data retention и test fixtures. Секреты в отдельном vault, не в event payload/логах, least-privilege credential broker. Сначала собственные проверенные адаптеры; third-party untrusted code не исполнять в privileged API worker.

**Bot Studio compatibility:** исследовать его existing adapter SDK и SQL inbox/outbox как reference. Переносить только neutral protocols, а не Telegram-specific bot dialogs/flow engine, с contract tests и независимыми product PR.

## Workers — кто исполняет?

- **Продуктовые workers** выполняют бизнес-операции конкретного продукта (Bot dialog flow, Planner background tasks).
- **Platform connector workers** исполняют только заявленные интеграционные действия (MAX/Telegram/Instagram/webhook).
- **Platform Jobs-as-a-Service** может позже выполнять ограниченные подписанные задания как общую способность для небольших продуктов; отдельный API/SLA/security isolation ADR.
- **Infra** обеспечивает CPU/RAM/disks/network, supervision/healthchecks/rollback, но не знает, что означает «опубликовать статью».

СГДС отвечает за durable delivery, Jobs — за фактический lifecycle задачи, Connector — за вызов провайдера, Registry — за возврат и историю результата. Развёртывание всех — через Infra с согласованием.

## Mail — три разных функциональности

1. Transactional email delivery (verification code, уведомления) → Platform email connector + Result Registry. Внешний provider может быть первым upstream.
2. Self-hosted SMTP relay/MTA (DNS SPF/DKIM/DMARC, reputation, bounces, anti-abuse, rotation, queue, rate quotas) → отдельная операционная способность с Infra как оператором и своим SLA/политикой; не смешивать с Identity.
3. Mailbox product: почтовые ящики/IMAP/webmail, приём пользовательских писем, поиск, квоты, data retention → самостоятельный продукт со своим репозиторием и backend, если появится потребность.

Ни SMTP ACK, ни provider accepted не гарантируют inbox placement или прочтение. Никаких open relays. Сама Identity лишь может инициировать письмо, но не должна становиться mail-сервером.

## Platform vs независимые продукты

Простой продукт может использовать Platform для restricted backend operations без своей полноценной API/worker инфраструктуры. Сложный продукт может иметь свой API/DB и использовать только Delivery/Connectors. У Platform нет полномочий хранить абсолютно все продуктовые данные; доступ к каждому connector должен контролироваться per-account/per-tenant. Нет обязательного межпродуктового proxy для каждого GET/POST.

**Следующие ADR:** delivery/ack semantics, broker vs DB log, operation status machine, job runtime isolation, credential broker, integration SDK, result contracts, provider capability/legal constraints, RPO/RTO и multi-node criterion.
