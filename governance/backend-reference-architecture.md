# E7A Backend Reference Architecture — PostgreSQL-first

**Статус: OWNER-APPROVED RECOMMENDED BACKEND BASELINE (2026-10-30), runtime НЕ реализован этим документом.** Этот документ описывает **предпочтительную архитектуру** новых stateful продуктов E7A, а не обязательный стек для Browser-only; альтернативы допустимы по обоснованному ADR. Основные инварианты утверждены в [Governance](README.md); инфраструктурные изменения — отдельный [Infra gate](infra-operations.md).

## Краткое решение

**Рекомендация:** `TypeScript-first + Node.js + PostgreSQL-first + explicit SQL migrations` для Full-stack и stateful Service/Worker. Начинать с **модульного монолита внутри продукта**, а не с десятков микросервисов. Не создавать один общий PostgreSQL с общими правами/схемами для всей экосистемы.

**PostgreSQL** — целевая СУБД, **`psql`** — клиент для администрирования и проверок. Не путать СУБД с CLI. Точная поддерживаемая major-version выбирается Infra/компонентом по lifecycle, совместимости и backup tooling (можно ориентироваться на уже используемый PostgreSQL 17, но не объявлять единой mandatory версией без миграционного плана).

### Логическая схема (Full-stack)

```text
Client (Browser / Mobile / API consumer)
      │ TLS
      ▼
Host edge proxy / WAF / traffic limit  [Infra; no product business logic]
      │ loopback / private network
      ▼
Product API (TypeScript/Node, e.g. Fastify; NestJS optional via ADR)
      ├─ HTTP boundary: input validation, safe errors, versioning, authN
      ├─ Application: use cases, policy (Actor+Org+Product+Capability), idempotency
      ├─ Domain: invariants, aggregate/operation state, domain ACL
      ├─ Persistence: SQL repositories + migrations / PostgreSQL transaction
      ├─ Outbox: durable deferred events (within SAME DB transaction)
      ├─ Adapters: Identity OIDC, Platform, Audit, Billing by need only
      └─ Jobs: bounded self-owned worker or versioned Platform trusted Jobs
      │
      ▼
Dedicated PostgreSQL DB/schema+roles for this PRODUCT
      │
      ├── migrations owner (separate high privilege)
      ├── runtime role (restricted DML)
      └── backup/export role (restricted)
```

`gateway.e7a.ru` personal WireGuard control plane is **not** this generic HTTP edge gateway and must not be placed in front of E7A products.

## Reference stack and criteria

| Layer | Recommended default | Permitted alternative / decision rule |
| --- | --- | --- |
| Language | Strict TypeScript (no blanket `any`), Node.js supported LTS | Different language only when domain, team and support justify an ADR |
| Transport | HTTP JSON REST/OpenAPI, explicit versioning, correlation/request IDs | gRPC/events as necessary; no compulsory gateway hop |
| Framework | Lightweight Fastify + schema validation (e.g. JSON Schema/Zod) | NestJS (potentially Fastify adapter) for larger teams/modules; do not prescribe framework org-wide |
| SQL access | PostgreSQL `pg`, SQL-first repositories, parameterized queries | Type-safe query builder/ORM when it preserves explicit migrations/tenant checks/observability; **no auto-sync in production** |
| Migrations | Ordered SQL, migration ledger + checksums, staging replay, operator-controlled apply | Another deterministic migration tool with the same invariants |
| Background | Process-local job only for disposable ephemeral work; durable product outbox+worker for state | Platform trusted Jobs by explicit contract; not privileged arbitrary user code |
| Cache | No cache until measured need; explicit invalidation/TTL | Redis only after evidence and security ownership |
| Observability | Structured redacted logs, traces/correlation IDs, health/readiness, business-safe metrics | OTEL-compatible exporters if feasible; never log PII/secrets by default |
| Auth | Mature existing OIDC IdP where required, JWT/JWKS/session validation, server-side policy | Anonymous guest for supported products; no homemade OAuth/crypto |
| Packaging | Immutable artifact digest, lockfile, containers or other reproducible release | Browser-only static assets don't require an API/database |

**Core `@e7a-ru/server`** provides neutral reusable types/helpers but is not a server, framework or dependency that may centrally operate product databases. No undeclared direct DB access between products.

## Data boundaries — PostgreSQL-first

1. **Every stateful product owns its DB/credentials/migrations/data and release**. The initial infrastructure can be one shared **PostgreSQL server/cluster** for resource efficiency, but production users/database roles are distinct and cannot read/write each other's tables. Do not conflate physical cluster with shared logical business database.
2. PostgreSQL managed by Infra (backup, upgrades, TLS/network, capacity, replicas if needed), domain schema managed by product. Prefer separate DBs, owners and runtime/migration roles; no `public` grants as a substitute for isolation. Validate privileges via negative tests.
3. `UUID` or explicit stable identifiers, `TIMESTAMPTZ` in UTC, `JSONB` for bounded versioned flexible data (not a schema-less substitute), `INET` for IPs when domain fits, foreign keys, `UNIQUE`, `CHECK`, indexes, transactions, constraints.
4. For multi-organization isolation: tenant/org scope required on **all relevant queries**, authorization at application boundary, unique constraints scoped appropriately, destructive/cross-tenant test cases; RLS is optional additional defense after ADR and tests, not a replacement for policy checks.
5. State transitions in the same transaction when possible: business rows + revision/CAS + idempotency receipt + local product audit event + outbox envelope. Serializable isolation, row locks or scoped advisory locks must be chosen based on tested concurrency and documented lock order. **No distributed exactly-once claim.**
6. Idempotency key scope includes **Actor/Organization/Operation/Request digest** and TTL/response rules. Duplicate same payload returns original result; same key/different payload is conflict. Always bind approval to exact plan hash, expected revision, Actor/scope and time window.
7. Outbox relay uses leases/retries and at-least-once delivery. Consumers need inbox/dedup and explicit `PENDING / APPLIED / FAILED / UNKNOWN / RECONCILE` outcomes; no external event is included in a local PostgreSQL transaction.
8. Migrations are versioned, checksummed and replay-tested against disposable databases; separate migration role and release gate. Use expand/migrate/contract across rolling releases; no implicit product start `AUTO_MIGRATE` in production.
9. Backup is **off-host, encrypted, access-controlled and restore-tested**. Establish RPO/RTO, retention, PITR/WAL when justified, secret/key restore dependencies and migration/data rollback behavior. No production data in local CI or Playground.
10. Secrets/DSNs never in Git, images, console dumps or logs. Product handles only its own credentials from Infra-controlled secret injection.

### Representative transaction pattern

```text
validate input + actor/tenant authority
 → BEGIN
 → lock/revision check & idempotency lookup
 → persist allowed domain change & new revision
 → persist local audit record + outbox event
 → persist idempotency result
 → COMMIT
 → asynchronous side effect/connector/privileged helper
 → verify external outcome; on uncertainty UNKNOWN + reconcile
```

Do **not** persist the final `SUCCESS` for a network/provider operation merely because the database transaction committed.

## Reuse Gateway design safely

Source reviewed from [Gateway PR #1](https://github.com/E7A-RU/gateway/pull/1) at `ead053cb6aa9fa573b50d3506e79fc2521131687` (2026-10-30). The project currently contains `apps/api/migrations/001_initial.sql`, `002_provisioning.sql`, `database.ts`, `provisioning.ts` and a pure `helper-contract.ts`. **These are IMPLEMENTED SOURCE, not verified passing runtime/test evidence.**

Useful ideas to generalize:
- **Dedicated PostgreSQL database** and runtime/migration credentials (Gateway supersedes its earlier historical SQLite production proposal; SQLite mock remains for test code).
- Product-owned SQL migration ledger, DB constraints, parameterized `pg` queries and transaction helper.
- Ledger of preview/approval/operation events, plan fingerprint, expected revision and short-lived approvals; unique device/peer constraints; safe error codes.
- Separate **pure request validator** vs privileged helper; do not infer capability authorization from shape validation alone.
- Read-only inventory → plan/preview → approval → apply → verify → reconciliation/rollback for high-risk external effects.

**Do not copy blindly:** Gateway's example uses a single revision lock and has development bearer-token access; it is not complete tenancy/MFA, durable privileged authorization, resilient audit or production-ready backend framework. Its current code cannot guarantee atomic host changes or full rollback. Review approval expiry at every execution boundary, stable canonical plan hashing and concurrency on exact PostgreSQL snapshot before extracting any shared library.

**What belongs where:** neutral types and codecs → Core after independent release; trustworthy operation semantics/jobs/connector results → Platform (when actually implemented); product-specific WireGuard/network operations → Gateway; privileged host execution → Infra and reviewed helper; security actor claims → Identity; durable protected audit → Central Audit.

## Architecture decisions needed per backend

- [ ] `BACK-001` Product runtime/framework/modular boundaries and versioned API schema.
- [ ] `DB-001` PostgreSQL major version, product DB/roles/schema/owner, SQL migration tool, backup/PITR, RPO/RTO.
- [ ] `TEN-001` tenant/org IDs, resource ACLs, actor and authorization, RLS decision and negative cases.
- [ ] `OPS-001` state transitions, idempotency, outbox/inbox, retries/UNKNOWN and reconciliation.
- [ ] `AUTH-001` OIDC subject mapping, session/MFA, service principals, recovery and logout/revoke semantics.
- [ ] `AUD-001` local transactional audit + central event mapping, outage/degradation policy and retention.
- [ ] `REL-001` resource budget, migrations, startup/readiness, staging/restore and rollback.
- [ ] `PRIV-001` privileged operation helper only where strictly required, never web app root/shell.

These are **implementation ADRs**, not blockers for a genuine no-backend Browser-only product.

## Acceptance before runtime

- [ ] Exact versioned dependencies + frozen lockfile and local/self-hosted TypeScript/build/test/lint/dependency/secret scans.
- [ ] Postgres temporary DB migration replay, checksum/drift, isolation/privilege tests, unique/index and concurrent transaction cases.
- [ ] Restore from encrypted **off-host** snapshot; migration/version compatibility, PITR when required.
- [ ] Auth/tenant permission negative tests, duplicate/replayed request and exhausted resource tests.
- [ ] Outbox worker crash between COMMIT and external ack; `UNKNOWN` reconciliation.
- [ ] Network/host privilege boundaries tested where applicable; server doesn't inherit admin/root.
- [ ] Infra staging checked with digest/exact SHA; production only through separate owner approval.

Related: [three Kit implementation roadmaps](kit-implementation-roadmaps.md), [lifecycle](product-lifecycle-checklists.md), [Platform](platform-delivery-jobs-integrations.md), [Identity](identity-and-trust.md), [Infra](infra-operations.md).
