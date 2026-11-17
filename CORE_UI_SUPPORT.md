# E7A Core UI — release and support window (2026-11-18)

**Canonical implementation/release contract:** [Core UI supported versions](https://github.com/E7A-RU/core/blob/main/docs/ui-supported-versions.md) ([pending Core docs PR #59](https://github.com/E7A-RU/core/pull/59)); this public note is a status summary for E7A repositories, **not** a new normative Governance standard or instruction to auto-upgrade any product.

## Release and supported versions

- **`@e7a-ru/ui@0.5.0` — CURRENT / SUPPORTED / PREFERRED.** Owner-authorized public npm upload completed on 2026-11-18 with `NPM_UPLOAD_ACCEPTED=PASS` and exact `PUBLIC_NPM_READBACK=PASS`. Core verified source `896113bcc93d7bfbd9e80b01e72844fd608786e3`; archive SHA-256 `7bb02bf2e88875ac9f5198d126a24f90cc4df333112149c38d44c0076c8bf179`. Includes approved autonomous E7A Wordmark v3 `wordmark-dark.svg`/`wordmark-light.svg`.
- **`0.4.0` — PREVIOUS / LIMITED SUPPORT** for verified pinned consumers during migration and bounded rollback. Existing Website dependency currently pins 0.4.0; this does not mean its public site already uses 0.5.0.
- **`0.3.x` and older — ARCHIVED / UNSUPPORTED** for feature work, security patches, compatibility promises or new deployments. npm copies that were actually published remain accessible and immutable; no unpublishing or rewriting. Any exceptional rollback is product-owner-approved, documented and time-limited.

**Owner rolling policy:** default current plus one previous minor release iteration, at most two previous minor iterations when a specific migration exception is approved. The explicit **minimum supported version is 0.4.0**, superseding any theoretical support for 0.3.x as second-previous. Re-evaluate the window with every new minor release; no indefinite `0.x` LTS.

**Adoption != publication.** Each application controls its own exact version and lockfile, independently tests npm archive/integrity, UI/accessibility/browser and rollback, then explicitly approves its own deploy. Core's 0.5.0 npm release does not deploy Website, Reader, Bot, Identity, Infra or other services. Product-specific historical version numbers in README files must not be misread as shared UI dependency pins.

The UI package is `UNLICENSED` and the official E7A Brand assets need advance written rights-holder permission for third-party use. See [Brand Usage Policy](BRAND_POLICY.md) and the [Core release evidence](https://github.com/E7A-RU/core/blob/main/docs/release-evidence/ui-0.5.0-prepublication.md).

**Open work:** [Core follow-up issue #58](https://github.com/E7A-RU/core/issues/58); [Website PR #90](https://github.com/E7A-RU/website/pull/90) will adopt 0.5.0 only after independent package and visual QA, followed by a manual Website-only release.
