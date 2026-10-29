# Architecture Consistency Report — 2026-10-25

**Status: REVIEW / PROPOSED remediation.** Scope: governance PR #1, its baseline main, and documented cross-repository relationships. This is a documentary architecture audit, **not** runtime certification or a comprehensive source-code audit of every consumer. Baseline main `5921f99dbce40caeb673c21811941836ff43f826`; initial PR head `47f32d74b18da9a30d996ae0df83a189034173eb`. The final head must be recorded separately after changes.

## Executive conclusion

Conceptual boundaries are coherent: independent products; shared Core UI/server libraries and optional Starter; Identity/Platform/Audit/Billing in early approved foundation scope but not verified RUNNING; Laboratory QA and release evidence; Infra controlled apply. The major systemic risk is **promoting documentation into presumed runtime reality**, followed by accidental mandatory dependencies and premature shared-service construction. B2C-first M1 must remain measurable while approved F1 includes bounded Identity, Platform, Audit and Billing; capital-intensive expansion beyond F1 requires evidence.

## Classification and conflicts

In the table: source → problem → consequence → proposed resolution → affected repositories → required approval. Ratings indicate severity of **potential** impact, not a verified production incident.

| ID / severity | Source and contradiction | Consequence | Proposed resolution | Affected repositories | Approval |
| --- | --- | --- | --- | --- | --- |
| AC-01 / High | [Charter](ecosystem-charter.md), [ownership](ownership-and-data.md): canonical Identity as future requirement vs [Reader](scenarios.md) anonymous/local-first | Forced registration breaks browser-only kit | Subject identity is canonical **when registration is needed**; anonymous flows remain first-class | .github, reader, future identity, core | Owner + Reader |
| AC-02 / High | [Product Factory](product-factory-and-kits.md) defines QA gate while [Infra](infra-operations.md) applies changes | Laboratory could accidentally acquire production powers | Two independent authorization steps: QA/evidence approval and time-bound Infra production apply; separate credentials | .github, laboratory, infra | Owner + Security |
| AC-03 / High | [Platform](platform-delivery-jobs-integrations.md) is a future capability; Bot already owns integration workflows | Cross-product shared backend or migration by assumption | Keep Bot product boundary; move only capabilities justified by consumers and explicit ADR | .github, bot, future platform | Bot owner + owner |
| AC-04 / High | [Core](ownership-and-data.md) includes server contracts and Starter, risking interpretation as common running backend or duplicated Kit engines | Tight coupling and duplicate generators | Core owns library distribution and currently optional Starter; Laboratory composes specs, **does not fork Starter** | .github, core, laboratory | Core + Laboratory |
| AC-05 / High | [Standards](standards-and-change-control.md): historical consumer Standard v1 copies vs desired unique canonical source | Drift and conflicting normative authority | Freeze historical copies, point consumers to exact immutable governance revision via separate PR; no bulk sync | .github, all consumers | Per-product owners |
| AC-06 / High | [Ownership](ownership-and-data.md): shared physical PostgreSQL instance vs independently owned product schemas | Cross-tenant access and blast radius | Distinct roles/DB credentials/backups; product owns domain model, Infra owns hosting only | infra, bot, future identity/platform, products | Security + data owners |
| AC-07 / High | [Laboratory](product-factory-and-kits.md) proposes runner on Lobachevsky, which hosts other workloads | Untrusted PR workload can disrupt KS54, prod Docker and secrets | Sandbox without prod Docker socket; CPU/RAM/disk/network quotas, dedicated identities, locks, retention, auditable cleanup; require explicit host approval | laboratory, infra | Infra operator + owner |
| AC-08 / Medium | [Market entry](market-positioning-and-entrypoints.md) and [ownership](ownership-and-data.md) contain outdated audience-priority-as-hypothesis text; [strategy](growth-strategy-and-first-milestone.md) fixes B2C-first | Ambiguous priorities | B2C-first is owner decision; B2B remains secondary route; individual value propositions still hypotheses | .github, website | Owner |
| AC-09 / High | [Status declarations](README.md): APPROVED concept vs merely PR Draft; `IMPLEMENTED` vs `RUNNING` | False investor/user-facing claims | Every component annotated with approved/proposed/implemented/tested/running evidence per revision | .github, all products | Owner + reviewer |
| AC-10 / High | [Integration operations](platform-delivery-jobs-integrations.md): requests across MAX/Telegram/Instagram are not transactional | Double sends, partial state and false exactly-once promises | Idempotency keys, bounded retries, DLQ, provider receipt, UNKNOWN state and per-target result subscriptions; no global exactly-once | future platform, bot | Platform owner + Security |
| AC-11 / Medium | [Brand Policy](../BRAND_POLICY.md), [Brandbook](../brandbook/README.md) and future merch/public packages | Exposure of public assets mistaken for permission to use E7A official branding | Preserve prohibition absent prior written authorization and lawful descriptive-use nuance; do not call UI open-source without a grant | .github, core, website, brand assets | Owner + legal |
| AC-12 / Medium | [Trademark strategy](../legal/e7a-combined-mark-first-filing-strategy.md) vs historical two-filing examples and class-25 merch proposals | Misstated rights, fees, class protection | One combined class-42 application is selected planning scenario; search and fees require fresh verification; EA7 class-25 risk remains; no filing or payment | .github, brandbook | Owner + qualified counsel |
| AC-13 / High | [Standards](standards-and-change-control.md) proposes automated compliance and changes, but hosted CI forbidden | Unexpected Actions spend or unapproved merges | Self-hosted/local checks only; R0/R1 auto-merge permitted only after a separately approved and proven risk gate; never auto-deploy; publisher only on separately authorized package workflow | all repos | Owner |
| AC-14 / Medium | [Team](team-and-capacity.md): 4.0 FTE role allocation vs independent approvals and backup recovery | Single-person critical control despite nominal separation | Identify at least two independent humans for sensitive gates and reserve operator; do not imply actual hires | laboratory, infra, organization | Owner |

No historical ADR should be deleted for these findings. Mark decisions `SUPERSEDED` only after explicit replacement and owner sign-off. Unknown implementation and evidence are **UNKNOWN**, not failure and not PASS.

## What was reviewed and limits

The PR already included charter, ownership, Platform/Identity, Laboratory, Infra, standards, demo, growth, 4.0 FTE, legal and brandbook documents, plus contract samples. `main` contained a lightweight public profile before PR. Review of linked **other repositories' source/runtime** is not certified by this document. There is **no evidence here** of running Identity, Platform, fully operational Laboratory, automated standards checker, complete QA sandbox or deployment changes. No production access or local Lobachevsky runner was invoked.

## Acceptance gate for future closure

- Every canonically required repo/policy has one named owner and an implementation status with evidence reference.
- Resolve AC-08 editorial conflict without rewriting past decisions.
- Link each consumer to an owner-approved immutable governance SHA and test at that consumer's exact head via separate PR.
- Validate Markdown links, syntax examples and no secrets on **final** PR SHA using safe read-only local scripts.
- Record risk acceptance or an approved mitigation per High finding.
- Keep PR Draft, no merge, production apply, Actions CI or package publication without separate authority.
