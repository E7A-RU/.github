# M1 delivery priorities and explicit decision gates

**Status: PROPOSED execution backlog.** [Final owner baseline](final-architecture-baseline-2026-10-27.md) establishes early foundation and recovery targets; the historical M1 task numbering below is not a current exhaustive engineering plan. Новые решения владельца 2026-10-27 в [консолидированном baseline](owner-decisions-2026-10-27.md) имеют приоритет над устаревшими вариантами ниже. Owner-approved strategic direction is **B2C-first**, B2B second. Reader is a possible flagship, **not selected**. This is not evidence of tasks completed, approved headcount or operating services.

## Ранний фундамент — принятое изменение очередности

Перед расширением продуктовой линейки: контракт и первый интеграционный вертикальный срез **Identity, Platform (Delivery/Jobs/Connectors/Results), Central Audit и bounded Billing Foundation**; при этом Website развивается параллельно. F1 проверяет живой end-to-end slice, F2 — отдельную эксплуатационную готовность, M1 — доказанную полезность B2C продукта. Список из десяти задач ниже остаётся историческим планом M1 и **не является единственным актуальным порядком реализации фундамента**. Пересобрать инициативы в отдельные PR после утверждения релизной версии Governance.

## Product-in-Use definition

M1 is met only when there is (a) one independently deployable B2C product delivering an observed user job-to-be-done, (b) reachable Website→Demo→product flow without simulated feature claims, (c) at least one consent-respecting feedback mechanism, (d) measurable activation/usefulness and quality/error signals, and (e) a documented **second** safe product release including rollback rehearsal or verified rollback readiness. Measure real use rather than downloads alone.

Suggested preliminary measures (targets require product discovery): successful onboarding and first task completion; repeat use at 7/30 days when measurable/privacy-appropriate; task success rate, error/crash rate, accessibility; feedback volume/severity and median response time; deployment recovery time and QA defect escape rate. For local-first Reader, privacy-preserving opt-in feedback and aggregate events are preferred; do not upload document contents.

## Next 10 practical tasks

| # / priority | Outcome | Dependency | Done when |
| --- | --- | --- | --- |
| 1 / P0 | Owner reviews [consistency findings](architecture-consistency-report.md) | Governance PR review | High findings assigned owners and dispositions |
| 2 / P0 | Approve governance baseline SHA/semver policy | #1 | Canonical revision immutable, Brand Policy unchanged |
| 3 / P0 | Select M1 product by user discovery (Reader is candidate) | Founder + user evidence | Written selection ADR, alternatives and primary user task |
| 4 / P0 | Verify Website→Demo funnel against deployed reality | #3 and website operator | Routes, content and simulation notices evidence recorded |
| 5 / P0 | Verify product MVP and privacy/data boundary | #3 | One end-to-end user task, explicit no-login/offline acceptance if Reader |
| 6 / P0 | Safe QA and release path using **local/self-hosted** tools | #2, Infra guardrails | Exact-head checks, independent review, immutable artifact digest |
| 7 / P0 | Controlled staging and restore/rollback rehearsal | #6 + authorized Infra resources | Resource budgets, health/backup recovery evidence; KS54 unaffected |
| 8 / P1 | First real B2C acquisition cohort and feedback loop | #4-7 | Measurable consenting user behavior and actionable feedback |
| 9 / P1 | Repeat release without regression | #8 | Second release passes QA and production authorization independently |
| 10 / P1 | Consumer-by-consumer governance adoption PRs | #2 | Core, Website, Reader, Bot manifests pinned and per-repo tested; no mass merge |

## Gates and dependencies

- **G0 / Strategy:** distinguish owner-approved B2C priority from unvalidated acquisition claims.
- **G1 / Discovery:** nominate a concrete user task and candidate product; cancel unproven features.
- **G2 / MVP:** independent product repo, Core UI only if valuable, anonymous browser execution allowed; basic manual QA acceptable with retained evidence.
- **G3 / Launch:** Website links and safe Demo, HTTPS, owner + independent release reviewer, backups where stateful, rollback plan.
- **G4 / Product-in-Use:** actual feedback and product use, second safe release; no fictitious metrics.
- **G5 / Shared services expansion:** Identity, Platform four contours, Central Audit and bounded Billing **already enter foundation scope**; only advanced expansions, operational scale and orchestrators wait for observed demand.

## What can remain manual at M1

A human release gate, a versioned checklist, manual support intake, limited user research, simple static catalog and manual provisioning with logs are acceptable. Non-negotiable: no secrets in test runners, separate prod permissions, privacy boundaries, real rollback, reproducible artifact and truthful demos. Never use GitHub-hosted Actions CI; package publishing is a separate explicitly authorized exception only.

## Owner decisions pending

1. M1 flagship choice, discovery sample and success thresholds.
2. Named people for Product, QA reviewer, Infra operator and backup; **4 FTE is an indicative capacity model, not staffed reality**.
3. Production capacity/support promise and target jurisdiction/privacy review.
4. Exact F1/F2 acceptance thresholds and independent reviewer capacity; Identity, Platform, Audit and bounded Billing are already approved foundation priorities.
5. Ratification of governance baseline and treatment of each architectural exception.

Related: [growth strategy](growth-strategy-and-first-milestone.md), [team](team-and-capacity.md), [demo](demo-and-trials.md), [governance standards](standards-and-change-control.md).
