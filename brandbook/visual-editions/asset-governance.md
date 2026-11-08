# E7A Visual Editions — Asset, UX and Release Governance

**Status:** draft procedural specification; no release authorization. See the [proposed Core-compatible integration contract](integration-contract.md): `data-e7a-theme` remains Dark/Light while a distinct opt-in `data-e7a-edition` identifies decorative Silver/Ember; **no such runtime edition API has shipped**. Applies to [Silver](silver.md) and [Ember](ember.md). Canonical source: [Brandbook](../README.md), [Brand Usage Policy](../../BRAND_POLICY.md), official [Core E7AMark](https://github.com/E7A-RU/core/blob/main/packages/ui/src/E7AMark.tsx), [E7ALogo](https://github.com/E7A-RU/core/blob/main/packages/ui/src/E7ALogo.tsx) and tokens.

## Ownership and change authority

| Area | Accountable owner |
| --- | --- |
| Editorial direction, restrictions and review | .github / brandbook + E7A brand owner |
| Logos, marks, canonical token sources, UI component API | Core |
| Product user preferences and visual accessibility | Individual consumer product |
| Showcase publication | Website owner |
| Synthetic preview / design QA | Admin Template owner; Laboratory only after its real QA capability exists |
| Merch samples / legal and rights clearance | Brand owner and relevant rights/manufacturing reviewers |

A documentation PR does not approve a design-master, introduce a normative Governance release or permit publication. All production rollouts are separate owner-reviewed PRs/releases under repository-specific policies.

## Recommended artifact layout (future, not created by this document)

```text
brandbook/visual-editions/
  README.md
  silver.md
  ember.md
  asset-governance.md
  assets/
    silver/<revision>/source/
    silver/<revision>/exports/
    ember/<revision>/source/
    ember/<revision>/exports/
```

Avoid storing font binaries, unlicensed third-party imagery, personal/private information or generated pseudo-official marks. Keep large binaries in a separately approved storage/release location if appropriate; retain references in a manifest.

## Proposed asset manifest fields

Each export, once it exists, should record:

```yaml
schemaVersion: 1
edition: silver # silver | ember
editionRevision: 0.1.0-study
role: wallpaper-lock # wallpaper-home | desktop | editorial | merch-mockup
status: concept # concept | candidate | approved | released | retired
sourceAssetSha: null # full pinned Core commit SHA required for candidate+
canonicalMark: E7AMark # or E7ALogo; omit only if no mark is shown
masterPath: null
exportPath: null
pixelSize: null
colorSpace: sRGB
fileSha256: null
authorAndRightsEvidence: null
brandApproval: null
consumerApproval: null
qaEvidence: null
licenseAndDistributionScope: internal-study
```

These keys are a documentation proposal, not a deployed parser or API contract. No `candidate` or later status without actual immutable source and evidence.

## Status transition gates

1. **Concept:** direction and mood study; generated art may contain inaccurate lettering; label it clearly and do not redistribute as approved logo asset.
2. **Candidate:** layer approved official exported mark from an exact immutable Core revision over the artwork; lock rights/provenance, editable source, export specs and checksums; provide side-by-side brand comparison.
3. **Approved:** review readability, color use, logo geometry, artboard safe areas, accessibility, mobile/desktop crops, legal rights. Record approver, timestamp and scope.
4. **Released:** separate platform/product publish approval, signed-off versioned exports and destination, consumer-specific QA, rollback strategy. No auto-publish from documentation merges.
5. **Retired:** preserve revision history and an explicit replacement or fallback.

## Digital deliverables and verification

- Wallpapers: separate lock/home layouts (portrait and desktop where appropriate), native output candidate `1206×2622` for iPhone 16 Pro; provide larger editable master and test Dynamic Island, clock/widgets, icon legibility, zoom/depth effects and crop on actual device.
- UI decorative skins: keep Dark/Light as semantic theme; opt-in only; no redefining keyboard focus, errors, warnings, success, user text, links or brand point. Test contrast, keyboard navigation, screen readers and reduced motion.
- UI compatibility: no migration of preexisting preferences unless separately designed/tested; provide a resilient fallback when an edition asset is missing or fails to load.
- Performance: asset size and decode/render costs measured in real consumer context; no required network call for local-first Reader.
- Review evidence: before/after snapshots at mobile/desktop, design audit of official mark, responsive test notes, accessibility findings, rollback/revert plan and checksums. A PASS claim requires actual run data, not this checklist.

## Merchandise and rights

Digital RGB palette is not a physical Pantone/CMYK/embroidery specification. Merch requires color proof, material/technique tests, dimensions, wash/wear review and rights approval. The existing class-25/trademark caution noted in [merchandise brief](../merchandise-and-hoodie.md) still applies; no production/sales authorized.

## Dependency and rollout discipline

- No cross-product brand changes merely because a Visual Edition is documented.
- No Core release until API scope, optional import behavior, versioning and consumer QA are established.
- Consumer chooses per-product feature-flag/preview/persistent preference contract, independently reviewed with a revert path.
- Website showcase, public downloads and commercial production each require their own explicit approval.
- Revisit at the next planned Core design or Admin Template UX iteration rather than adding an urgent workstream.
