# Матрица владения E7A и границы данных

**Статус: согласованная целевая архитектура, не перечень запущенных сервисов.**

| Компонент | Что принадлежит | Что не принадлежит |
| --- | --- | --- |
| Core | @e7a-ru/ui, @e7a-ru/server, reusable contracts, pack/release tools, текущий Starter | Живые аккаунты, платежи, сервисный runtime, CI platform |
| Laboratory | Wizard, матрица Kit/QA, evidence SHA, sandbox specification, release approvals | Production apply, продуктовая бизнес-логика, копии исходников Core |
| Infra | Hosts/VM, compute quotas, volumes и резервирование, Nginx/TLS/DNS, service rollout, runners и Docker GC | Domain ACL, личные данные, product DB schema |
| Identity | Person/subject, assurance claims, tenants/organisations, memberships, SSO, ecosystem product entitlements, developer identities | Боты, книги, задачи, платежные детали других продуктов |
| Platform | Durable Delivery, Jobs, connector accounts scoped by product, integrations, results после реализации | Universal shared business DB, identity issuer |
| Website | Сайт/маркетинг/каталог/SEO | Корневые credentials, глобальная маршрутизация API |
| Bot Studio | Сценарии диалогов, integrations/chat, API, PostgreSQL, RabbitMQ, worker и tenant ACL | Общая identity authority или все внешние действия E7A |
| Reader | Локальные файлы и RSVP, browser state | Обязательная общая БД/логин, загрузки книг по умолчанию |
| Planner | Будущие планы/задачи/собственный API по необходимости | Хранение задач внутри Identity |
| Mailbox product (если появится) | Почтовые ящики и пользовательские письма, права, retention, SLA | Просто SMTP adapter в СГДС |

## Где «общая база»?

Единой пользовательской БД всего E7A **не планируем**. Каждая граница данных имеет владельца и миграции:

- **Identity DB**: канонические ID, связи людей/организаций, подтверждённые атрибуты, access grants и аудит.
- **Platform DB/storage**: durable inbox/outbox/operation receipts, leases/jobs, dedup, результат и политики retention; не читать напрямую базу Identity.
- **Bot DB**: аккаунты интеграций Bot, чаты, сценарии, Inbox/Outbox; самостоятельная tenancy.
- **Продуктовая DB**: только если продукту нужна серверная бизнес-персистентность. Reader local-first БД не требует.
- **Infra**: выделяет физические storage/DB/volume, но не превращается в доменного владельца; credentials, backup, restore принадлежат политике владельца данных и оператору.

На одном Lobachevsky несколько DB могут физически размещаться в одном PostgreSQL-кластере **только** с отдельными DB users/roles, схемами, ограниченными полномочиями и понятным blast radius. Выделенные инстансы под Identity/Platform — решение после оценки критичности и стоимости. У всех stateful-сервисов потребуются off-host backups, проверка восстановления, retention и RPO/RTO. Нельзя считать snapshot той же машины отказоустойчивостью.

## Кто принимает решение о доступе?

- Identity: кто субъект, подтверждён ли атрибут, есть ли общая подписка/право входа в продукт.
- Продукт: можно ли редактировать **этого** бота, читать **эту** книгу, изменить **эту** задачу и войти в **эту** команду.
- Platform: разрешено ли данному subject/service account использовать *конкретный* connector/credential, target и operation.
- Infra: разрешено ли operator делать deploy/provisioning. Не путать со входом пользователя в E7A.

## Пограничный пример

Петя зарегистрирован в E7A, подтверждён email и имеет доступ к Bot Studio: это Identity. Петя может менять конкретного бота только в рамках tenant/role Bot. Запуск webhook от имени команды требует Platform capability и scope. Сервер с этим ботом может жить на Lobachevsky, но Infra не владеет сведениями о Пете или сценариями бота.

Кросс-продуктовые запросы только через согласованные API/events/SDK, никогда не через общую DB credential.
