# E7A Games — самостоятельное продуктовое направление

**Статус: OWNER-REQUESTED PRODUCT DIRECTION / PROPOSED SCOPE; NO REPOSITORY, GAME OR RUNTIME CREATED.** Added to the ecosystem catalog in Governance PR #1; genre, audience, monetization, product owner, first title and launch timing are **OPEN**.

## Роль

**E7A Games** — отдельное направление разработки цифровых игр, интерактивных развлечений и игровых продуктов под экосистемой E7A. Игры могут быть B2C flagship candidates, но **не назначаются автоматически** первым M1-продуктом. Games — направление/портфель, а отдельная игра становится продуктом только после утверждения Product Charter; она должна иметь собственные границы данных, релиза и ответственности.

## Границы ответственности

- Games: game design, gameplay, content, assets, progress/saves, achievements, matchmaking/multiplayer **только когда требуется**, game-specific moderation, economy and product KPIs.
- Core: optional shared UI, design tokens, common SDK/type contracts and Starter; **не** универсальный игровой движок и не обязательный rendering/runtime API.
- Identity: optional registered-player login, Person/Account mapping, organization/context and access grants; **публичные, гостевые и офлайн-игры допустимы без Identity**. Never expose real-world verified identity by default to game peers.
- Platform: optional background Jobs, notifications, allowed Connectors/Delivery/Results; **not** an authoritative gameplay state store or mandatory real-time game loop.
- Billing: game pricing, purchase/subscription entitlements and provider references after legal/payment readiness; player wallet, virtual currencies and in-game economy require a **separate legal/security/product ADR**, not implied by Billing Foundation.
- Audit: security/privileged/financially relevant events via common envelope; game telemetry and player behavior remain product-owned, privacy-bounded and subject to separate consent/retention.
- Laboratory: suitable Kit/Starter and CI-free self-hosted QA, isolated preview, age/content/accessibility tests and release evidence. External engine/tool selection is product-owned after compatibility, licensing and brand review.
- Infra: hosting, networking, backup and controlled releases where needed. Browser-only and local games may not require dedicated servers.

## Game-specific risks and gates

1. Audience, geography, age suitability and children/privacy protections must be decided before public sign-up or monetization.
2. Licensing/ownership of game art, music, engines, fonts, UGC and third-party assets is independent of E7A Core and Brand Policy.
3. Online multiplayer, real-time sessions, chat, moderation, anti-cheat and abuse handling are capabilities to introduce **only for a product that needs them**. Do not require generic Platform to provide them.
4. If prizes, randomized paid rewards, user-to-user transfers, virtual assets, or chance-based monetary mechanics are considered, require explicit legal review and owner decision; none is presently approved.
5. No auto-created repo `games`, no new services, no trademark subclass promises, no downloads or production deployments are authorized by this charter.

## First decisions still needed

- Is Games primarily a **portfolio/division** of independent game products, a **named flagship game**, or a **shared game publishing/studio platform**? Default proposal: portfolio/division, not a universal gaming platform.
- First target: browser casual/educational/utility games, mobile games, desktop/console, multiplayer? Do not assume genre or game engine.
- Is Games eligible for M1 selection through the same discovery criteria as Reader? Proposed yes.
- Decide content rating, child audience, monetization, distribution stores and assets/license before an implementation PR.

Related: [charter](ecosystem-charter.md), [ownership](ownership-and-data.md), [final baseline](final-architecture-baseline-2026-10-27.md), [M1](growth-strategy-and-first-milestone.md).
