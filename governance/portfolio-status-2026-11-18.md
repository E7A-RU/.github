# Публичная сверка E7A — 18 ноября 2026

Это обновление фактов публичного профиля и навигации. Оно не является
новой версией Governance, полным аудитом исходников или host-аудитом.

| Объект | Подтверждение | Предел вывода |
| --- | --- | --- |
| Governance | [Descriptor в publication commit](https://github.com/E7A-RU/.github/blob/5c11fd53dc3b203ce5166187cc10d564ad3f2c1d/governance/standards-release.json): 4.0.0, source `abba7788f305bbf4d0494d8b34c82aeea197a2ee` | Адресная процедура Identity; нет массового adoption |
| Core UI | [Публичные npm-метаданные 0.5.0](https://registry.npmjs.org/@e7a-ru/ui/0.5.0), HTTP GET 18.11.2026: version 0.5.0, license UNLICENSED, dist.integrity присутствует | Проверены метаданные, не скачивание/тестирование архива и не установка у потребителей |
| Reader | [Публичный manifest](https://reader.e7a.ru/e7a-product.json), HTTP GET 18.11.2026: released, 0.1.4, publishedAt 2026-11-03 | Подтверждает метаданные доступного endpoint, не полный функциональный smoke |
| Website | [Публичный manifest](https://e7a.ru/e7a-product.json), HTTP GET 18.11.2026: released, 0.1.0, publishedAt 2026-11-03 | Product version/date не равны дате последнего deployment |

Исправлены устаревшие заявления о Core UI 0.2.1 как текущем релизе,
Reader как ещё не запущенном продукте и Governance 3.0.0 как неопубликованном
предложении. Не добавляются обещания готовности Identity, Planner или других
направлений для публичного использования.

[Срез 31.10.2026](portfolio-status-2026-10-31.md) сохранён как история;
его таблицы и следующие шаги не являются текущим списком работ.
Детальный внутренний source-аудит и карта репозиториев предназначены
для приватного профиля участников.

Публичный профиль: `.github/profile/README.md`.
Профиль участников: приватный `.github-private/profile/README.md`.
[Официальный механизм GitHub](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile).

Проверки этой правки — локальные проверки Markdown, ссылок и scope;
результат и точный head SHA фиксируются в PR. GitHub Actions, выпуск пакетов,
обновление зависимостей и production не запускались.
