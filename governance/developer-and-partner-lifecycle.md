# Laboratory — вход для команды, партнёров и продуктовой разработки

**Статус:** предложенная продуктовая модель поверх уже согласованного Wizard+QA+Release Gate. Portal, HR-system, external developer admission, клиентский кабинет и отдельные разрешения ещё не реализованы.

## На какие задачи отвечает Laboratory

**Задача владельца 1 — пополнять инженерную команду:** создавать контролируемый маршрут от заявки или приглашения до проверки навыков и присоединения к E7A.

**Задача владельца 2 — создавать конкретное решение заказчику:** из потребности клиента и согласованного scope получить проверенный продукт или интеграцию, не собирая монолитные патчи в центральном backend.

Laboratory — центральная **среда инженерного производства**, но не общий код всех продуктов, не сайт рекламы, не произвольная SaaS-песочница с доступом к реальным клиентским аккаунтам и не система для неконтролируемого выпуска.

## Три разных входа и доверительные границы

| Роль/маршрут | Что разрешено по умолчанию | Что только после проверки |
| --- | --- | --- |
| **Applicant (кандидат в команду)** | Публичная информация об участии, анкета/контакт через разрешённый intake; при приглашении — учебная задача в изоляции | Доступ к private repo, системам пользователей, production, корпоративным данным |
| **External developer / Partner (B2D/B2B)** | Документация публичных API, безопасная demo, запрос участия | Identity SSO/MFA, договорённости и scope, project-scoped repo/QA workspace, connector credentials |
| **Employee / Staff (B2E)** | Вход через Identity с membership и role; рабочий проектный портал | QA approve, deploy approve, elevated Infra access, break-glass (разные полномочия) |
| **B2B customer** | Website/catalog/demo и проектная заявка | Ограниченный client project workspace, статус согласованного проекта/релиза по audience, без доступа к исходникам других клиентов |
| **B2C visitor** | Публичный Website и анонимные демо/продукты | Авторизация для платных/private capabilities конкретного продукта |

Владелец может одобрить pilot invite-only режим. **Рекомендованный безопасный default:** никакого автоматического self-signup в привилегированные разделы Laboratory; внешний и внутренний маршруты разделены и проверяются через Identity. Не считать B2B-клиента автоматически разработчиком.

## Пример A: пополнение команды

Website «Сотрудничать» → согласованная форма/контакт (privacy notice) → review кандидата и назначение тестовой задачи → Laboratory создаёт isolated training project на синтетических данных через Infra → проверка QA и review → решение руководителя → Identity создаёт/обновляет membership и ограниченные права → assigned product repo + short-lived tokens. Отказ или завершение участия → отзыв прав, аудит, сроки хранения данных кандидата. **Не копировать чужие private repo/secrets в испытание.**

## Пример B: проект B2B «интегрировать заявку из CRM в MAX и Telegram»

Website → запрос автоматизации → scope/смета/возможности официальных API → проектное согласование → Laboratory Wizard выбирает Service/Worker Kit или Platform Connector capability → staging со stub-провайдерами → QA exact SHA, API/contract tests, leakage/timeout/idempotency → customer acceptance → Infra controlled deployment → Result Registry сообщает статус отправок согласно подтверждениям провайдеров. Клиент видит только собственный project report, разработчики — нужные версии и логи без персональных данных.

Если customer workflow не нуждается в самостоятельном backend, Platform по утверждённым контрактам может предоставить ограниченные capabilities; если нужна сложная доменная логика, backend остаётся в отдельном репозитории продукта.

## Пример C: новый разработчик создаёт продукт

Identity / developer enrolment → Laboratory Wizard → один из трёх Kit (Browser-only, Full-stack, Service/Worker) из **Core Product Starter** → отдельный repo и pinned standard SHA → QA self-hosted runner → Demo Preview/staging через Infra → reviewer approval → самостоятельный release. Не дублировать Starter и не превращать Laboratory в платформу GitHub Actions.

## Неподвижные границы

- Core: версии библиотек и шаблонов.
- Identity: субъект, атрибуты, проверки и membership; **не** результаты собеседований в общем пользовательском профиле без privacy basis.
- Laboratory: проектная спецификация, QA, evidence, approvals, team/developer workflows.
- Infra: isolated runner, compute, staging, prod apply после отдельного одобрения.
- Platform: опциональное исполнение typed jobs/integrations/results; не HR-движок по умолчанию.
- Продукт: доменные данные и самостоятельный release.

HR/recruitment case management, CRM и юридический workflow могут со временем потребовать **собственного независимого продукта**, а не разрастания Laboratory. Начальная версия может быть минимальным intake с явным владельцем данных и согласованным хранением.

## Метрики будущего продукта

Onboarding lead time; доля успешно завершивших изолированное испытание; time to first safe contribution; repeatable QA rate; defect escape; среднее время первого staging; доля релизов с полным evidence; % deployment с подтверждённым rollback. Метрики личных кандидатов собирать пропорционально цели и сроку хранения.

## Открытые вопросы

- Внешние developers и организации заходят в Laboratory после приглашения/договора или допускается открытый self-service sandbox?
- Кто владелец recruitment/lead records и сколько хранятся сведения о кандидатах?
- Где B2B customer видит ход проекта: в отдельном client workspace Laboratory или самостоятельном customer portal?
- Разрешается ли external partner публиковать сторонние коннекторы, и кто проводит security review?
