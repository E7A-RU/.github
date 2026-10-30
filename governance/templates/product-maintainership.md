# Product-local MAINTAINERSHIP — E7A template

> **TEMPLATE.** Fill within *the product's own repo* at `docs/MAINTAINERSHIP.md`. This is a handoff index and evidence ledger; not a PR approval, product release or runtime declaration. Normative principles: [E7A Product Stewardship](../product-stewardship.md) and [Governance](../README.md).

## Product and version

- Repository: `E7A-RU/<name>`
- Purpose and boundary:
- Product Kit: `Browser-only | Full-stack | Service/Worker | Engineering/Infra only | TBD`
- Governance: **v1.1.0**, immutable normative SHA `c74bce10bc9eac6bb2e604a7db6b14d33b7c7645`.
- Local `governance-consumer.json` state: `MERGED | DRAFT | PENDING`
- Actual status: `DIRECTION | DESIGN | IMPLEMENTED_SOURCE | TESTED_EXACT_SHA | RUNNING_VERIFIED | UNKNOWN`
- Last verified source SHA and evidence date: `UNKNOWN` unless read/verified.

## Accountability and agent delegation

| Responsibility | Current assigned identity or evidence | Notes |
| --- | --- | --- |
| Accountable organization owner | E7A Owner | Normative change and production approval |
| Product Owner | `TBD — owner assignment pending` | Scope, product roadmap, acceptance |
| Engineering lead / VP agent | `TBD — assign per work session` | Implementation, README/AGENTS, PR handoff |
| QA reviewer | `TBD / temporary owner-only exception` | exact-SHA QA evidence, no invented independent reviewer |
| Infra operator | `TBD — separate approval` | Execution and production recovery |
| Security/privacy reviewer | `TBD / owner risk decision` | Secrets, data, tenant boundary, law |

**Do not fill TBD with guessed names or indicate that an AI agent has human normative ownership.**

## Entry points for next agent

- `README.md`: product boundary and current documented version.
- `AGENTS.md`: repo-specific work rules; if not yet in `main`, identify the Draft PR containing it.
- `docs/governance-adoption.md` or `docs/architecture/governance-adoption.md`: local interpretation, not duplicate organization standard.
- `docs/status.md` or verified operations ledger: source and running state are separate.
- Product Charter / ADR, current Draft implementation PR and tests.
- [E7A Governance](https://github.com/E7A-RU/.github/tree/main/governance) · [Kit roadmap](https://github.com/E7A-RU/.github/blob/main/governance/kit-implementation-roadmaps.md) · [Release checklist](https://github.com/E7A-RU/.github/blob/main/governance/templates/release-gate.md).

## Open PR tracker

| PR | Purpose | State / evidence | Merge gate |
| --- | --- | --- | --- |
| `<url>` | `<scope>` | `DRAFT/READY/BLOCKED` | `<exact SHA tests and approvals>` |

Other responsibilities and pending dependencies:
- `<product-specific needs>`
- `<cross-product contracts and versions>`

## Standard acceptance for next PR

- [ ] Live `main` and Draft PR head SHA read, ownership/policy checked.
- [ ] Existing related PR continued, no duplicate PR or force push.
- [ ] Diff contains no unapproved packages, workflows, brand, secrets, external system actions.
- [ ] Documentation: relative links/JSON validate, stale claims corrected.
- [ ] Code: frozen install, strict types, lint, test/build/audit and negative scenarios at exact SHA **with real output**.
- [ ] Privacy/security and product-specific lifecycle gates satisfied or marked `BLOCKED/UNKNOWN`.
- [ ] Product owner has approved scope; production requires **separate** owner + Infra approval.
- [ ] Handoff and PR body updated with PASS/FAIL/SKIPPED/UNKNOWN, timestamp and next step.

## Production and recovery (only observed facts)

- Is there a deployed service? `NOT_VERIFIED`
- Current deployed artifact SHA/digest: `UNKNOWN`
- Secret owner/location (redacted): `TBD`
- Backup/restore RPO/RTO: `NOT_VERIFIED`
- Support/recovery and rollback: `TBD`
- **No deployment is authorized by this file.**

## Next sprint

1. `<P0 action>`
2. `<P1 action>`
3. `<questions requiring human owner approval>`

Recheck status when next agent starts. A dated document is not continuous monitoring.
