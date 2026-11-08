# E7A Visual Editions — Visual Study Register (2026-11-08)

**Status: NON-CANONICAL VISUAL REFERENCE / NOT A PUBLISHED ASSET.** These entries describe two newly generated concept boards and two initial phone wallpapers. The raster binaries are **not stored in this repository** by this documentation change; the names below are provenance identifiers, **not** resolvable paths, download links, released masters or approved assets. No statement of a published license or approved redistribution is implied.

## Study inventory

| Study | Edition | Working source filename (outside git) | Canvas | Purpose |
| --- | --- | --- | --- | --- |
| SILVER-BOARD-01 | Silver | `a_polished_cinematic_brand_reference_banner_for_e.png` | 1672 × 941 | Wide visual art-direction board: low-noise graphite, satin arcs, silver edge light, registration lines, small blue accent and editorial title |
| EMBER-BOARD-01 | Ember | `a_sleek_cinematic_brand_style_board_for_e7a_ember.png` | 1672 × 941 | Wide art-direction board: faceted metal, one contained warm seam, engineering geometry, contrasting blue brand reference and editorial specifications |
| SILVER-PHONE-01 | Silver | `E7A_Silver_iPhone16Pro.png` | 1206 × 2622 | Initial iPhone 16 Pro wallpaper study; composition and lock-screen friendliness are provisional |
| EMBER-PHONE-01 | Ember | `E7A_Ember_iPhone16Pro.png` | 1206 × 2622 | Initial iPhone 16 Pro wallpaper study; composition and lock-screen friendliness are provisional |

Canvas sizes were inspected from available image files in the design conversation; they **do not** prove device-fit, color accuracy, or brand acceptance. Before importing or publishing any of these binaries, copy them through a separately reviewed asset workflow and record an actual checksum and rights/provenance evidence in the [asset manifest](asset-governance.md).

## Findings from visual design review

**Silver:** controlled cool metallic luster and broad curves are aligned with [Quiet Precision](silver.md). A dark negative-space region supports a quiet brand placement. The board's depicted upper-left wordmark and upper-right seven are **generated approximations**, not official E7ALogo/E7AMark. Technical glyphs are ornamental, not authentic manufacturing diagrams.

**Ember:** angular planes, contained orange illumination and distinct blue highlight are aligned with [Active Geometry](ember.md). Orange is atmospheric, not semantic/status, and must not replace Core's theme-aware balance-point. The board's depicted wordmark and seven are also **generated approximations**, not approved marks. Do not copy any rasterized brand lettering as the basis of vector artwork.

**Important correction:** one board illustrates “compatible: Dark / Light” as an editorial claim. The actual [integration contract](integration-contract.md) deliberately permits decoration **only in Dark** until a separate Light treatment passes QA. The images are not authoritative for behavior, token values or compatibility. Any printed hex/text in a generated image must be independently retypeset from the source specifications.

## Canonicalization workflow

1. Recover/recreate a licensed editable backdrop master for each edition; maintain an unbranded art-only layer. No rasterized pseudo-logo in the master.
2. Pin a full 40-character Core source commit SHA for the official E7ALogo/E7AMark geometry; render/export the real mark unchanged, including its theme-aware balance point. Record mark export SHA-256.
3. Typeset all editorial copy separately using approved typography, verify terminology/hex strings against source specs, and check contrast/crop.
4. Export a revisioned board (desktop), lock-screen and home-screen mobile variants with explicit safe-area/device reviews. Artboard dimensions by themselves are not QA evidence.
5. Document `status`, source rights, version, role, source and output hashes, owner sign-off and distribution scope in the [manifest rules](asset-governance.md).
6. Only after independent approval may these be linked publicly from Website or used by a consumer. Store the approved binaries in a versioned authorized location; this documentation records direction, not distribution.

## Relationship to the existing brand

[E7A Brandbook](../README.md) and Core's actual SVG/CSS sources remain authoritative. Existing merchandise concept boards in `../references/` are independent studies. The names Silver / Ember never replace the primary Dark/Light UI themes and do not authorize the manufacture/sale of physical products.

This register supplements [PR #22](https://github.com/E7A-RU/.github/pull/22) and [Issue #21](https://github.com/E7A-RU/.github/issues/21); it introduces no runtime implementation or production files.
