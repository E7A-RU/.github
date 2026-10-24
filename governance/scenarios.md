# Примеры применения архитектуры E7A

**Все сценарии ниже — иллюстративные архитектурные примеры, если явно не сказано иное.** Не подтверждают существование runtime или доступность внешних API.

## Пример 1. Создать analytics.e7a.ru «с кнопки»

Разработчик в Laboratory Wizard выбирает **Full-stack**, фиксирует frontend/API/healthcheck, нужен E7A Identity, PostgreSQL да, Redis нет, quota CPU=1 и RAM=512MiB (пример, не фактическая резервация). Laboratory создаёт spec и новый независимый репозиторий через Core Kit; QA на self-hosted runner проверяет коммит и образ; Infra формирует plan: isolated Compose project, loopback port, vhost/DNS/TLS, secrets, database role, backups, metrics, staging. Владельцу показывают разницу desired/current. **Только отдельный approval разрешает apply**. В дальнейшем при реальной multi-node необходимости возможен K3s adapter. Это не автоматическая реакция на любой push.

## Пример 2. Три Kit

- **Browser-only:** Reader загружает PDF/DOCX/EPUB/FB2 в память браузера, не отправляет книгу в E7A backend и не требует SSO для чтения. В будущем opt-in cloud sync — отдельное решение и privacy review.
- **Full-stack:** Planner хранит личные задачи в собственном API/DB, login у Identity, доменная авторизация остаётся Planner; может использовать Platform Delivery для асинхронного уведомления.
- **Service/Worker:** Bot Studio со своим API, сценарием диалога, PostgreSQL+RabbitMQ и workers; вправе подключаться к Platform Delivery, но не переводится туда автоматически и не становится всей интеграционной платформой.

## Пример 3. Одна персона, разные разрешения

Петя имеет E7A subjectId и verified email. Identity подтверждает membership в организации A и entitlement в Bot Studio. В Bot он оператор бота «Продажи», но **не** администратор чужого бота. В Laboratory он может смотреть QA своего проекта, но production approve — отдельная capability. Кредитные, паспортные данные и членства не рассылаются всем приложениям. Если персональные атрибуты подтверждены другим методом, раскрываются с provenance/assurance и политикой доступа.

## Пример 4. «Отправить Пете MAX-файл, Васе Telegram, опубликовать Instagram-пост»

Planner подписанным запросом направляет batch из 3 actions в Platform. СГДС возвращает **Accepted + batchId + operationIds** после durable записи. Dispatcher распределяет по специализированным connector workers. MAX file, Telegram message и Instagram post имеют разные capability constraints и client-consent. При результате MAX AcceptedByProvider, Telegram AcceptedByProvider, Instagram Failed из-за недостаточных разрешений — итог **PARTIALLY_SUCCEEDED**, а не «всё доставлено». Для всех разрешённых инициаторов доступны GET status и подписанный result webhook; запрещены утечки в чужие tenants. См. [Delivery и интеграции](platform-delivery-jobs-integrations.md) и [пример JSON](../contracts/operation-batch.example.json).

## Пример 5. Подтверждения не должны обманывать

Email провайдер вернул SMTP 250 — подтвердил приём на следующем hop, но это не доказательство входящих или прочтения. Webhook consumer вернул 200, но нет договора, что бизнес-транзакция закоммичена — это не Processed. При timeout Telegram отправка могла произойти, а ответ потеряться; состояние **UNKNOWN**, без слепого повторения.

## Пример 6. Workers против СГДС

СГДС надёжно поставила команду «создать отчёт». Product-owned worker строит PDF, сохраняет в своём хранилище и **сам** подтверждает результат. СГДС может доставить callback, но не отвечает за корректность отчёта. Shared Platform Jobs возможен позднее только для типизированных безопасных заданий.

## Пример 7. Почта

Код подтверждения Identity направляется через Platform email connector к внешнему provider, а не через собственный IMAP в Identity. Если E7A решит держать SMTP relay, Infra разворачивает отдельную контролируемую службу с SPF/DKIM/DMARC и мониторингом репутации. Полноценная E7A Mail с почтовыми ящиками/IMAP/webmail — самостоятельный продукт, не обязательный кусок Platform.

## Пример 8. Названия профессорских узлов

Lobachevsky — текущий физический хост с несколькими Docker-проектами и отдельными Compose namespaces. Новые хосты/VM именуются фамилиями учёных по центральному registry Infra и не должны привязывать домен или имя продукта к железу. Дополнительная виртуализация/K3s на одном узле не создаёт CPU, RAM или физический диск, лишь меняет управление и накладные расходы.

## Пример 9. Эксплуатация без регресса: Docker GC на общем сервере

В рамках KS54 проверяли GC на Lobachevsky: общий deploy flock, проверка активной сборки, dry-run, сохранение running/stopped container image IDs и rollback tags, раздельная обработка release images и shared edge-tags, запрет volume prune. Первая контролируемая очистка увеличила доступный диск примерно с 97 до 165 GiB; критические контейнеры остались healthy. **Это исторический пример практики**, не означает, что тот же скрипт должен без аудита применяться ко всем E7A продуктам. Infra должна разработать общую owner-aware политику retention и логирования WARN/SKIPPED.

## Пример 10. Изменился единый стандарт

Оператор предложил изменение в организационном governance через PR. Laboratory сверяет pinned standard revision у consumer manifest и на self-hosted runner строит compatibility matrix. Потребителям создаются **предложения PR**, но не массовые прямые изменения. Для Core/UI — отдельные immutable версии; Dependabot лишь открывает PR, exact SHA тестируется; approval владельца требуется для merge и deployment. Для поломки контрактов — migration window/ADR. Ссылки в README на latest без pinned revision не заменяют проверку.
