# E7A Demo — безопасная витрина и пробные сценарии

**Статус:** предложенная продуктовая возможность. Общего Demo Catalog, trial-control-plane или нового demo.e7a.ru как подтверждённого production-сервиса нет. Домен/URL определяется отдельно после DNS/TLS approval.

## Зачем Demo

Посетитель должен увидеть способности E7A **до покупки или заключения договора**; клиент — подтвердить применимость к своей задаче; разработчик — испытать ограничения и контракты. Демо не обязано запускать полноценный backend на каждого гостя и не должно расходовать production ресурсы бесконтрольно.

**Demo — это не новый универсальный монолит.** Это сквозной режим и каталог интерактивных примеров, у которых есть владелец, лимиты и измеримые outcomes.

## Слои и ownership

| Уровень | Роль | Owner |
| --- | --- | --- |
| Discover / Demo Catalog | Публичное описание, фильтрация, примеры, ограничения, CTA | Website |
| Demo Scenario | Чему учит и что демонстрирует: sample documents, mocks, scenario versions, expected outcomes | Конкретный продукт / его команда |
| Experiment UI | Запуск управляемого сценария для developer/partner, sandbox preview и QA | Laboratory |
| Sandbox Provisioning | Изоляция, TTL, quota CPU/RAM/disk, автоматический cleanup, logs и network policy | Infra |
| Identity / Trial Account | Один subject для зарегистрированного trial, scoped access/consent, сроки истечения | Identity после запуска |
| Backend capabilities | Mock/replay, limited delivery/test connector operations, safe status API | Platform после реализации, или продуктовая test stub |
| Outcomes | Демонстрационный результат + CTA к продукту или проектному запросу; без ложных success claims | Product + Website |

## Три режима Demo

1. **Public interactive showcase (анонимно):** без персональных данных и приватных учётных записей, ограниченные ресурсы, явно синтетические данные. Например открытый Reader sample и статическая/локальная симуляция workflow.
2. **Guided demo (по запросу):** демонстрация целевого B2B-сценария оператором, согласованный набор безопасных тестовых данных; не подключать живые аккаунты клиента без отдельного consent/security review.
3. **Scoped trial / Developer sandbox (по приглашению, с E7A Identity):** время жизни/лимиты, изолированная среда, ревокация/cleanup, аудит; никакого production доступа по умолчанию.

## Пример 1: «Попробовать Reader»

Website показывает ссылку на Reader demo, пользователь загружает свой локальный файл **не на сервер**, а в браузер. Для публичного примера можно предоставить не содержащий ПДн synthetic документ. Нет обязательной авторизации, обязательного storage/books upload или Platform Delivery.

## Пример 2: «Как E7A автоматизирует коммуникации»

Пользователь видит сценарий: «Пете через MAX — файл, Васе через Telegram — сообщение, в Instagram — пост». Demo показывает **симуляцию** Accepted → Dispatched → Delivered или UNKNOWN/FAILED/NeedsApproval, ID операций, частичный итог и способ получить callback/status. В демо коннекторы используют mock adapters, synthetic identities и sandbox media. **Никакие реальные MAX/Telegram/Instagram публикации не производятся от имени гостя.** Реальные provider connections доступны только в отдельном проверенном customer/trial workflow и после подтверждения ограничений официальных API.

## Пример 3: «Сделать продукт за несколько шагов»

Показываем Wizard: пользователь выбирает Browser-only / Full-stack / Service-Worker, Identity и опциональные backend capabilities; получает пример product manifest и результаты учебных QA-checks. Публичная демоверсия **не создаёт GitHub-репозиторий, DNS, БД или контейнер**. У доверенного developer есть отдельное Laboratory sandbox после приглашения и approval.

## Пример 4: демонстрация под B2B-запрос

Компания просит автоматизировать обработку заявки. Website объясняет возможные результаты, Laboratory на тестовых данных собирает proof-of-concept, Infra предоставляет staging, Platform test connector возвращает статусы без реального внешнего side effect. После scope и approval проект получает production-план. Демонстрация не гарантирует, что все внешние каналы доступны в нужном tenant.

## Безопасность и экономика

- Synthetic/demo data only; отсутствие production secrets, shared DB credentials, mutable production API access и неконтролируемого egress.
- Quota по аккаунту/IP/tenant; rate limit, TTL, storage cap, антиспам и optional CAPTCHA/abuse mitigation.
- Прозрачная маркировка **Simulation / Real Sandbox / Production** и provenance результата; mock success не является доказательством доступности провайдера.
- Автоочистка sandbox после истечения срока, журналы без ПДн и токенов, минимальная retention.
- Скачиваемые документы/media проходят форматные/объёмные лимиты. Загруженные файлы пользователя не сохраняются скрытно.
- Внешние демо-ссылки доступны через Website/каталог, но выдача доступа к Laboratory/Identity/Platform отдельно и с least privilege.
- Нет бесплатных безлимитных фоновых workers, открытого SMTP relay или неограниченных API ключей.

## Roadmap до реализации

P0 — каталог потенциальных демонстраций и владельцев в Website, выбор первого proof (например Reader или безопасная симуляция SGDS). P1 — QA/synthetic fixture/contract + настоящий demo gate. P2 — управляемый sandbox с TTL/quotas и аудитом. P3 — guided B2B demo и приглашённый trial, измерение конверсии и полезности. Production demo endpoint, домены и стоимость требуют отдельного одобрения.

## Открытые вопросы

- Нужна ли единая публичная страница «Попробовать» или демонстрации только на страницах отдельных продуктов?
- Что первично: Reader/готовые инструменты для B2C, симуляция Integrations/SGDS для B2B или Wizard для разработчиков?
- Какие действия можно разрешать анонимно и какой бюджет sandbox?
