# E7A Ecosystem — единый архитектурный стандарт v1 (25.10.2026)

**Статус:** философия и зоны ответственности согласованы. Не означает работающие Identity/Platform/Laboratory runtime, завершённые Kit или разрешение на деплой. Существующие Draft PR проверяются на точном SHA перед merge.

**Историческая ссылка на раннюю консолидацию:** [E7A Infra — Operating Model](https://github.com/E7A-RU/infra/blob/docs/ecosystem-operating-model-20261025/docs/architecture/ecosystem-operating-model.md). **Новый канонический источник организации — [governance/README.md](governance/README.md)**. Этот файл сохраняется как snapshot первого согласования, а не второй нормативный текст.

## Роли

| Компонент | Назначение | Граница |

| --- | --- | --- |

| Core | Версионированные UI/SDK/server contracts и текущий необязательный Product Starter | Не работающий backend и не владелец данных/деплоя |

| Laboratory | Самостоятельный продукт: Wizard, QA, sandbox, проверка exact SHA, Release Gate/approvals | Не переписывает Core Kit, не делает production apply |

| Infra | Серверы/ВМ имени учёных (первый Lobachevsky), Compose/K3s при необходимости, сети, DNS/TLS, ресурсы, runner, storage, backup и deploy/rollback | Не владеет бизнес-данными и правами продуктов |

| Identity (будущий самостоятельный продукт) | Единый каталог зарегистрированных людей, разработчиков, администраторов, организаций и service accounts, OIDC/SSO, подтверждения и общие права доступа | Не ведёт domain ACL продуктов; нет обязательного входа для анонимных функций |

| Platform (RFC, будущий backend-as-a-service) | Ограниченные общие сервисные возможности для продуктов с собственным backend или без него; первый кандидат — СГДС | Не общая бизнес-БД, не монолит всего E7A |

| Продукт | UI, собственный backend/DB/worker при необходимости, доменные права и независимый выпуск | Никаких прямых записей в чужие базы |

## Три Kit

1. Browser-only: локальное приложение без обязательного API/БД (Reader).

2. Full-stack: UI + собственный API, при необходимости PostgreSQL и workers (например будущий Planner).

3. Service/Worker: backend API и/или фоновые обработчики без обязательного UI (например Bot).

Core пока владеет кодом Product Starter. Laboratory предоставляет выбор Kit, Wizard и QA, Infra — staging и эксплуатацию. Источник шаблонов не дублируем. Выделение Starter в отдельный lifecycle — только по ADR.

## Центральная Identity и данные

Для всех зарегистрированных субъектов E7A — единый неизменяемый идентификатор. Identity отвечает за SSO, членство в организациях, подтверждённые атрибуты и межпродуктовый доступ. Продукт решает предметные права и владеет своей БД. «Персона известна» не равно «личность доказательно подтверждена»: атрибуты имеют provenance, уровень доверия, срок и правила раскрытия. Scope, privacy, MFA, отзыв прав и миграции логинов проектируются отдельно; предпочтительно применять стандарты OIDC/OAuth 2.0. Анонимное чтение Reader и публичный Website не требуют принудительного входа.

Нет единой бизнес-БД E7A: Identity, Platform и каждый продукт владеют своими данными/миграциями. Infra только размещает базы, изолирует их ресурсы, делает backup/restore и мониторинг. На Lobachevsky базы физически могут находиться рядом при должной изоляции ролей и резервного копирования, но для критичных сервисов оцениваются blast radius, off-host backup и RPO/RTO.

## СГДС: внутренние и внешние каналы

Platform пока RFC. Первый исследуемый сервис — СГДС для внутренних сообщений и внешних webhook, email, Telegram/MAX через адаптеры. Accepted означает надёжную запись; Delivered — подтверждение приёма по контракту канала; Processed — подтверждение обработчиком, не абсолютную бизнес-истину. At-least-once delivery, идемпотентность, дедупликация, outbox/inbox, retries, DLQ, аудит, квоты и scoped credentials; exactly-once для внешних эффектов не обещаем. При потере единственного физического сервера без off-host durability абсолютной гарантии нет.

## Workers и mail — примеры границ

Продуктовые workers принадлежат продуктам (Bot владеет своими обработчиками). Platform может позже предоставить Jobs/Workers-as-a-Service с отдельным контрактом исполнения и подтверждения результата; Infra выделяет CPU/RAM и наблюдаемость. Решение об отдельном worker-продукте — только по нагрузке и самостоятельным SLA.

Транзакционная отправка email — outbound adapter СГДС. SMTP relay/MTA (SPF, DKIM, DMARC, abuse/bounce/reputation) — отдельный эксплуатационный сервис при реальной потребности; mailbox hosting с IMAP/webmail — отдельный пользовательский продукт. SMTP acceptance next-hop не равен доставке в inbox или прочтению. Собственный mail-хостинг не вводится автоматически.

## Примеры

- Website: самостоятельный публичный сайт; не общий API gateway и не хранитель учётных записей.

- Reader: browser-only и local-first; никаких серверных книг или обязательного входа без отдельного одобрения.

- Bot Studio: свой API, PostgreSQL, RabbitMQ, worker и tenant security; Identity лишь единый субъект и разрешение входа.

- Planner: будущий продукт; свой API/DB только при необходимости, а задачи и планы не принадлежат Identity.

- Небольшой продукт: может выбрать Platform backend capabilities вместо собственного backend, не теряя независимых данных/релиза.

## Release process и защита истории

Идея → Wizard Laboratory → Core Kit → независимый репозиторий → QA exact commit/artifact SHA на self-hosted runner → staging Infra → approval → контролируемый production deploy Infra → health/rollback/audit. GitHub-hosted Actions CI по текущей политике запрещён, Dependabot лишь создаёт PR. Не создавать автоматические реакции на push без отдельного ADR. E7A UI публичен, но UNLICENSED; бренд E7A — отдельная политика использования. Core #25, Website #57, Reader #7, Bot #6, .github #1 — существующие Draft, нельзя молча сливать/переписывать.

## Что ещё не утверждено технически

IdP и уровни подтверждения личности; модель SSO и миграций; формат декларации Kit, runner sandbox; storage/broker для СГДС, ACK/SLO, внешние адаптеры; worker execution contract; внешний почтовый провайдер против собственного MTA; DB restore drills и disaster recovery. Их реализация — отдельные ADR/PR и проверка.
