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

## Пример 11. Руководитель компании ищет автоматизацию под ключ (B2B)

Компания хочет убрать ручную передачу заявок из CRM в мессенджеры без ненужных затрат. Website объясняет сценарии и ограничения, даёт **симуляционный Demo** и безопасную заявку. После квалификации задачи и согласования бюджета Laboratory организует Service/Worker или Full-stack Kit, использует Platform capabilities только где они реально подходят; Infra создаёт staging, QA проверяет exact SHA, и production меняется только после разрешения. Результат измеряется корректностью процесса, поддержкой и прозрачной стоимостью, а не заявлениями «самое дешёвое решение». См. [позиционирование](market-positioning-and-entrypoints.md).

## Пример 12. Кандидат пополняет команду E7A (B2E после допуска)

Кандидат нашёл Website/ссылку «Сотрудничать». Intake-форма не даёт доступа к GitHub или серверу. После review получает только sandbox с синтетическими данными для тестового задания. Laboratory фиксирует QA, а администратор принимает решение о членстве. Только затем Identity выдаёт employee membership и минимальный project access; production deploy approve остаётся отдельным правом. См. [разработческий и кадровый маршрут](developer-and-partner-lifecycle.md).

## Пример 13. Внешний разработчик и партнёр (B2D)

Партнёр сначала изучает открытые контракты и демо, затем по приглашению проходит Identity и получает ограниченный Laboratory workspace. Выбирает Kit, запускает self-hosted QA через Infra, публикует проверенные изменения в собственном PR. Нельзя выдавать токены Bot/Platform и production-доступ всем self-registered гостям. См. [onboarding](developer-and-partner-lifecycle.md).

## Пример 14. Публичный пробник интеграций: MAX, Telegram, Instagram

Website предлагает «Посмотреть как работает». Публичная демонстрация формирует синтетическую группу операций и показывает Accepted, Delivered, FAILED/UNKNOWN и частичный итог; **ничего реально никому не отправляет и не публикует**. Настоящий trial требует согласия и доступа к конкретным provider accounts, проверки provider capabilities, rate limits, изолированного staging и подтверждённых полномочий. См. [Demo](demo-and-trials.md).

## Пример 15. B2C-гость испытывает Reader

Гость открывает ссылку из Website и локально пробует Reader на синтетическом файле или собственном документе в браузере, без обязательного E7A Identity, регистрации и загрузки книг на сервер. Если потребуется платная облачная синхронизация, это отдельное утверждаемое развитие Reader и privacy architecture.

## Пример 16. Путь к первой B2C цели M1

Посетитель e7a.ru видит понятный продукт и ограничения → открывает бесплатное безопасное демо → успешно выполняет основное полезное действие → сообщает о неудобстве → команда устраняет дефект, независимо проверяет exact SHA и проводит следующий release с rollback. Если выбран Reader, документы остаются в браузере. **Цель:** доказать повторяемую пользу для B2C, не «построить все сервисы заранее». См. [M1](growth-strategy-and-first-milestone.md).

## Пример 17. Минимальная команда

На первом B2C запуске Founder отвечает за product/budget, инженер — функциональность, дробные design, QA, SRE/security, growth и support дают около **4,0 FTE суммарной работы**, но не обязательно четыре работника. Для критичных production изменений необходим независимый review и backup operator. Это гипотеза capacity, не юридически утверждённые вакансии. См. [Team](team-and-capacity.md).

## Пример 18. Фирменное худи E7A

**Quiet Edition:** тёмная ткань цвета, сопоставимого с E7A bg; небольшой официальный E7AMark на груди и синий узел; спина без крупного принта. **Engineering Edition:** крупный официальный mark на спине и надпись SYSTEM 07 на рукаве, после визуальной проверки. Оба — референсы, а не существующие товары; нанесение, размеры, права, цветопроба, физические sample и тираж требуют утверждения. См. [Brandbook/merchandise](../brandbook/merchandise-and-hoodie.md).
