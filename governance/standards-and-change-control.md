# Единый стандарт: управление версиями и зависимостями

**Статус:** согласованная целевая governance-модель; автоматический compliance crawler/PR bot ещё не реализованы.

## Единственный нормативный источник

- E7A-RU/.github/governance — канонические организационные требования; BRAND_POLICY.md — самостоятельная публичная политика.
- Core — источник исполняемых UI/Server SDK/Starter и их immutable releases; другие продукты владеют собственными runtime и локальными дополнениями.
- В репозиториях-потребителях сохраняется **ссылка + pinned стандарт revision (commit SHA/tag)** и свои локальные правила, а не семь редактируемых копий документа. Исторический Standard v1, ранее добавленный в Draft других репозиториев, — snapshot и должен быть конвертирован в ссылку **отдельными PR после review**, с сохранением git history.
- Разногласия фиксируются в ADR с owner, expiry, compatible standard version и планом миграции. Продукт не вправе молча ослаблять обязательные политики.

## Как будет работать автоматическая сверка — только план

1. RFC/PR обновляет governance с changelog, impact map, владельцами и security review.
2. После approval публикуется неизменяемая версия или commit SHA; в consumers пока остаётся прежняя совместимая версия.
3. Self-hosted Laboratory checker получает manifests продуктов, сравнивает их pinned standard revision, SDK package versions, Identity/Platform API constraints и тесты совместимости.
4. Создаёт **предложения** отдельных Draft PR и reports, а не пушит в main и не включает массовый deploy.
5. Для каждого consumer необходим exact-head SHA test/log, package readback при adoption Core, тесты поведения и независимое owner approval.
6. Infra получает подтверждённый release manifest; staging/prod отдельно, продакшен требует явного разрешения, health и rollback.
7. Результат проверки различает PASS, FAIL, SKIPPED, UNKNOWN; пропущенный из-за lock процесс не является пройденным.

## Пример

Core выпустил новый UI. Website, Bot Studio и Reader имеют разные UI потребности и независимые lockfile; Dependabot предложил PR, Laboratory проверяет совместимость с UI, брендовый parity, browser/build и контрактные тесты. Результат — независимые PR и раздельные релизы; Core publish **не** перезапускает продукцию.

Аналогично, изменение протокола Platform Connector version не перезапускает всех пользователей: compatibility matrix определяет кто должен обновиться и в каком release window.

## Security и billing

GitHub-hosted Actions CI по текущей E7A политике запрещён. Единственное указанное исключение для Core — отдельно разрешённый ручной package publisher, без тестового CI. Dependabot предлагает PR, не сливает их. Не генерировать фальшивые GitHub checks или выдавать отсутствие CI за успешный gate. Нельзя открывать private secrets, токены, персональные данные или производственные endpoints в logs.

Публикация @e7a-ru/ui@0.2.1 указана подтверждённой публично, но license UNLICENSED; public npm ≠ open source. Отдельная Brand Policy ограничивает third-party official brand use, сохраняя правомерные factual references.

## Контроль изменений

Каждый PR: affected products, standards revision before/after, контрактные изменения, owner, QA evidence, security/privacy, rollout, rollback и возможность приостановки. Аргумент «привести всё к актуальному стандарту» не даёт права переписывать все репозитории или сливать чужие Draft.
