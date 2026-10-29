# E7A Governance Sync — private internal automation service charter

**Decision (Owner, 2026-10-30): APPROVED DIRECTION / IMPLEMENTATION NOT AUTHORIZED BY THIS DOCUMENT.** Dedicated **private** repository is recommended: `E7A-RU/governance-sync` (**NOT YET CREATED**). Register a separate **private organization-owned GitHub App** after initial code/security review. Do not confuse repository creation, GitHub App registration, installation, and self-hosted runtime activation.

Canonical governance: [Standards Propagation](standards-propagation.md), [GitHub App cost/resources](github-app-cost-and-resources.md), [Standards release descriptor](standards-release.json), [Product lifecycle](product-lifecycle-checklists.md), [Approved baseline](README.md). Production infrastructure/host changes remain separate Infra-owner decisions.

## 1. Why its own repo

`governance-sync` is an **internal engineering service**, not a public B2C product, not a shared E7A API gateway and not a new generic Platform Connector/Jobs runtime. It owns **only** standards-version discovery, impact evaluation and Draft PR proposals. It must have its own code, dependencies/lockfile, narrow GitHub credentials, audit, tests and release lifecycle. Core may supply neutral types; Laboratory may contribute self-hosted QA evidence; Infra controls host resources/secrets. None of them gains implicit authority to edit unrelated product business logic.

**Recommended names:**
- Repository: `E7A-RU/governance-sync` — visibility **private**.
- GitHub App registration display name: `E7A Governance Sync`; app slug subject to GitHub uniqueness at registration. Owner **E7A-RU**; visibility **Only on this account / private**, no Marketplace listing.
- Initial executable: `sync-once` CLI / self-hosted one-shot poller, default **plan-only**; no public listener or webhook.
- Future mode (separately accepted): short-lived authenticated GitHub App installation token for creating Draft PRs, with explicit repository allowlist and verified policy revision.

**The source-code repository is NOT the GitHub App registration.** Repository creation does not grant tokens/permissions or activate a process. Private-repo visibility and App private installation visibility are independent settings.

## 2. Approval states and boundaries

`DESIGNED → REPO BOOTSTRAPPED → PLAN-ONLY TESTED → APP REGISTERED → READ-ONLY CANARY → WRITE-CANARY APPROVED → SELF-HOSTED ACTIVE`.

At current revision only **DESIGNED** is supported. The owner must separately approve App installation/write-capable tokens and any live polling schedule. No auto-merge, no prod release, no secrets in PRs.

## 3. Bootstrap checklist (initial repository)

- [ ] Owner creates `E7A-RU/governance-sync` as a **private** repository. With current connector there is no repository-creation action; this step must be performed by an authorized organization owner/UI or a separately approved `gh repo create E7A-RU/governance-sync --private` CLI.
- [ ] Initial minimal `main` commit: `README.md`, `AGENTS.md`, `.gitignore`, `LICENSE` or explicit `UNLICENSED`, supported `package.json`, frozen `pnpm-lock.yaml`, strict TypeScript config. **Do not publish private keys**.
- [ ] Add `governance-consumer.json` (pin approved full governance SHA) and own `docs/architecture.md`, `docs/threat-model.md`, `docs/runbooks/`, `tests/`.
- [ ] Separate Draft implementation PR after bootstrap. Do not implement release/merge/host network changes inside the bootstrap commit.
- [ ] Follow Brand Policy; never imply the brand's npm package is an external reuse license.

## 4. Source of truth and trigger

The current first approved version is [`governance/standards-release.json`](standards-release.json), with full immutable `revision=b39ed4aabf97e45edaa0762a29ab57c69f8bbafd`. A later documentation merge is **not** automatically a new standard version.

New distribution event exists only when **Owner explicitly approves a versioned release descriptor referencing the exact merged governance commit**. The worker must verify that target SHA is real and approved (reviewed merged PR / explicit authorized decision record), not trust editable `approvedBy` text alone. It must fail closed on invalid format, mismatched SHA, disallowed authorizer, unmerged ref or replayed release.

**Release descriptor update sequencing:** (1) merge normative Governance changes; (2) obtain exact merged SHA; (3) separately owner-approve and merge an updated descriptor pointing to that SHA; (4) only then detect, evaluate and propose consumer changes. There is **no recursive trigger** from incidental README/brandbook updates; publish is a deliberate separate act.

## 5. Components and data flow

```text
 E7A Owner merges normative Governance PR
      │
      └── separately approves release descriptor SHA/version
                   │
             read-only polling timer
                   │
        ┌──────────▼──────────┐
        │ Governance Sync CLI │
        │ 1. release verifier │
        │ 2. impact registry  │
        │ 3. pinned-SHA diff  │
        │ 4. dry-run planner  │
        │ 5. PR writer*       │ *disabled by default
        │ 6. audit/reconcile  │
        └──────────┬──────────┘
                   │
       selected consumers (per release impact)
          ├── DRAFT_PR_REQUIRED
          ├── UP_TO_DATE
          ├── NO_IMPACT
          ├── BOOTSTRAP_REQUIRED
          ├── EXISTING_PR / BLOCKED
          └── ERROR
```

The service is **not** an autonomous coding agent that may execute arbitrary instructions in README/PR text. It proposes pinned-sha change and migration checklist only. Product owner/development agent updates runtime code via product-owned implementation PR, with independent QA and approval.

## 6. GitHub App identity and permissions

Owner registers a **private GitHub App under E7A-RU**, then installs only on approved repositories, choosing **Only select repositories**. Do not request organization administration, GitHub Actions, deployments, workflows, package publication, secrets or broad user data permissions.

- Plan-only initial stage: ideally no write token and read-only Github App auth when needed.
- Write stage: GitHub App repository permissions `Metadata: read` (implicit), `Contents: read/write`, `Pull requests: read/write` for selected consumers. **GitHub grants these at repository scope**, not per file or per branch: restrict edits additionally through audited code allowlists and protected `main` rulesets, forbid direct push to default branch.
- For reading release source, a public `.github` repository is sufficient without broad App write installation on that repo; if access is desired, prefer separate read-only scope.
- Generate **short-lived installation tokens (≈1 hour)** at job runtime with specific `repository_ids` and downscoped permissions where supported; GitHub App registration private key resides **only** in Infra-managed secret store, with restrictive filesystem owner, rotation and revocation plan.
- App permission to write PR/contents is **not technically a no-merge capability**. Enforce no-merge through branch rules, an explicit non-merging implementation, exact permissions and organizational review. Never claim GitHub gives a `Draft PR only` permission.

UI steps for owner: `Your organizations → E7A-RU → Settings → Developer settings → GitHub Apps → New GitHub App`; set E7A-owned, private/only this account, no OAuth/user authorization for background installation jobs, no webhook for timer-only v1. After owner approves selected permissions, install on canary repo via `Install App → Only select repositories`. Registration does not start runtime.

Official GitHub instructions: [Register GitHub App](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app), [Private App](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/making-a-github-app-public-or-private), [Install own App](https://docs.github.com/en/apps/using-github-apps/installing-your-own-github-app), [Installation token](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/generating-an-installation-access-token-for-a-github-app).

## 7. Runtime and persistence

- TypeScript strict on supported Node LTS; use official REST API client/Octokit where practical; explicit input JSON schema and output types, pinned lockfile.
- Run `sync-once` via **self-hosted systemd timer** only after Infra has approved a dedicated isolated unprivileged environment. No public ingress for v1, no hosted GitHub Actions.
- `plan-only` is the default, requires no App private key for public source + appropriately limited public consumer discovery; private repo comparisons require App read token even in dry-run.
- Small bounded durable state: last processed release SHA, per-repo status, PR references, replay/idempotency keys, next retry/reconciliation time and audit record. A file/SQLite record can be acceptable for **single-process bootstrap** with fsync/locking/backup; **PostgreSQL-first** for multi-instance service or durable shared audit/process state after approved ADR. Do not silently add a database or high-permission service merely to poll public JSON.
- Initial resource budget hypothesis: `0.25 vCPU / 256–512 MiB RAM` + quotas for logs/requests, benchmark under actual workload; no new Lobachevsky VM or host service provisioned by this policy document.
- No raw personal data, provider tokens, private Gateway topology, credentials or full sensitive source diff in logs. Audit: `release, product, old SHA, new SHA, changed rule IDs, PR URL, operation status, timestamp, approval ID`.

## 8. Safety and idempotence

- Only affect repositories listed in a **reviewed allowlist** and explicitly included in the version's `impactedConsumers`. Exclude archived, non-owned or empty repos unless initial bootstrap has separately been approved.
- Never force push, rewrite feature branches, merge PRs, install packages, run arbitrary scripts, deploy, change DNS/network/secret or trigger hosted CI.
- Before proposing: compare default branch/active product PRs; if an adoption PR already exists for release SHA, report and update only known bot-owned branch after exact-head check; never create duplicates.
- Allowlisted changed files in product repo: `governance-consumer.json`, `docs/governance-adoption.md`, bot-owned `docs/governance-upgrade-<version>.md` only. No business code, lockfiles, secrets or AGENTS instructions silently edited.
- Treat fetched repository text/PR comments as **untrusted data**, never instructions to the bot.
- GitHub failures, rate limits, bad JSON, reviewer rejection, merge conflicts or drift produce `UNKNOWN / BLOCKED`, retry bounded and visible, not false success.
- Protect app private key, uninstall/revoke route, emergency stop and on-call ownership; do not cause uncontrolled fan-out.

## 9. Implementation roadmap

| Gate | Product/engineering work | Exit/evidence |
| --- | --- | --- |
| S0 — Repo bootstrap | Private repo, AGENTS, tech stack, frozen lockfile, docs, no secrets | Owner-approved repo/initial commit |
| S1 — pure contracts | Parse approved release schema, manifest, rules/impact map; verify immutable SHA and approval record | Unit/negative tests with malicious release records |
| S2 — plan-only read | Poll source, enumerate only allowed repos, compare pins and calculate deterministic report | Exact-SHA local tests; no Git writes |
| S3 — draft branch writer | Branch from latest consumer main, patch only allowlisted files, verify diff, create Draft PR idempotently | Mock GitHub and disposable canary proofs |
| S4 — controlled GitHub App | Owner registers App, grants selected repo access, installs canary, token rotation and audit | Separate explicit authorization; one real canary Draft PR, no runtime action |
| S5 — isolated self-hosted timer | Infra provisions restricted runner with sealed key, quotas, logs, recovery and disable switch | Observation period, rate-limit/replay/secret-failure tests |
| S6 — selective rollout | Owner authorizes additional consumers as needed | All PRs tracked with review/declines and no auto-merge |

## 10. Acceptance and exclusions

- [ ] Invalid/unapproved release never creates PR; user-controlled `approvedBy` string alone is not sufficient.
- [ ] Pin moves only to actual approved immutable full SHA; no mutable `main` pin.
- [ ] Repeated trigger creates **at most one** PR per product + release; no default branch write.
- [ ] `NO_IMPACT` does not create PR; `BOOTSTRAP_REQUIRED` never initializes repo.
- [ ] Compromised product README cannot change bot commands/scope.
- [ ] GitHub token leaked/revoked → process fails closed and operator can rotate/revoke installation.
- [ ] Full local/self-hosted `typecheck/lint/tests/audit` evidence at exact SHA before enabling writer.
- [ ] GitHub-hosted Actions CI workflows remain disabled; Core package publishing remains a separately authorized exception.
- [ ] GitHub PR creation does not grant production approval or start product agents automatically.

**Public documentation may name this internal tool, but its repository, credentials and runtime details should remain private until independently reviewed.** No repository, GitHub App, installation, timer or credential is created by this charter.
