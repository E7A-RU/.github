# E7A Visual Editions — Silver / Ember

**Status:** DRAFT BRAND SPECIFICATION / OWNER-DIRECTED EXPLORATION, 2026-11-08. These are documented *secondary visual treatments*, not a new E7A identity, approved production assets, released themes or merchandise SKUs. Associated decision: [Brand Issue #21](https://github.com/E7A-RU/.github/issues/21).

## Purpose

Make experimental visual directions reusable in wallpapers, editorial imagery, presentations and future optional product surfaces without weakening the single E7A brand. Both editions are described at the level of design intent, palettes, forms, typography, responsive composition, motion, use cases, anti-patterns and review gates. Documented does not imply commercially approved or implemented.

## Three-layer model

| Layer | Authority | Responsibility | May editions override? |
| --- | --- | --- | --- |
| A. Canonical E7A | [Brandbook](../README.md), [Core UI](https://github.com/E7A-RU/core/tree/main/packages/ui), [Brand Policy](../../BRAND_POLICY.md) | E7ALogo, E7AMark, proportions, blue node, base colors and typography | **No** |
| B. Product UI themes | Core `@e7a-ru/ui` and consumer-owned QA | Current Dark and opt-in Light, semantic states, controls, a11y | **No** |
| C. Visual Editions | This proposed specification | Decorative surfaces, art direction, backgrounds, editorial/promo and opt-in skins | **Only within decorative scope, after approval** |

**Canonical reference palette (not new edition values):** `--e7a-bg #0B0D12`, `--e7a-text #F5F7FA`, `--e7a-muted #B8C0CC`, `--e7a-blue #3B82F6`. Source-of-truth values are imported from Core; these values are an observed snapshot, not independent authority. Manrope serves content; JetBrains Mono is reserved for short technical annotations.

The official `E7AMark` is a geometric seven with a square blue balance point. Do not fabricate new E7A lettering, change the seven, rotate/recolor the mark or replace the blue node. Current AI wallpaper studies contain **unofficial stylized lettering**: useful for atmosphere only; not official logo assets, not production ready.

## Edition index

| ID | Name | Character | Main imagery | Decorative accent |
| --- | --- | --- | --- | --- |
| `silver` | [Silver — Quiet Precision](silver.md) | Calm, exact, restrained | Graphite, satin-metal curves, light-traced edges | Neutral/cool reflective light |
| `ember` | [Ember — Active Geometry](ember.md) | Focused, energized, architectural | Angular gunmetal, structural cuts, directed illumination | Warm orange/red ambient glow; **never identity blue** |

Both are meant to feel like E7A, not like two unrelated subbrands. Prefer quality of material/light over high detail density. Names `Silver` and `Ember` label treatments, not products or registered marks.

## Edition behavior and delivery

- Default appearance remains unaffected. No global theme overrides, automatic migration, auto-enabled skins, saved preference resets or semantic token changes.
- Provide an explicit opt-in and one-action revert to Dark/Light if an edition is ever implemented as a product skin. Persist only within that product's approved preference contract.
- Editorial/wallpaper assets may be produced separately from UI implementation; designs must remain legible under crops, iOS lock-screen clock/widgets and home-screen icons.
- Use `prefers-reduced-motion`; keep decoration outside interactive hit areas and avoid confounding glow with focus, error, warning or success.
- Even high-resolution visual studies must not be called approved brandmasters until exact Core assets, origin, rights and QA evidence are recorded.

## Product ownership and non-goals

- **.github/Brandbook** owns this direction and brand review; **Core** owns canonical marks and any eventual shared *optional* decorative contract.
- **Website** may curate an approved gallery; **Admin Template** is a candidate first isolated synthetic preview; **Laboratory** could later gather preview/QA evidence once capability is real.
- **Reader, Bot, Planner, Gateway** can opt in individually after product-specific review. Identity, Platform and Infra require no visual-edition changes.
- No new repository, product dependencies, live accounts, UI package release, GitHub Actions CI, production deployment, merch manufacturing or sales are authorized here.
- Next review occurs at a **planned design-system / Admin Template visual iteration**, not as an urgent ecosystem rollout.

See the [Core-compatible integration contract](integration-contract.md) for the two-axis Dark/Light × none/Silver/Ember connection, scope/isolation, preference migration and exact acceptance matrix. See [asset and release governance](asset-governance.md) for master/manifest fields, stage gates, constraints and acceptance.

## Proposed naming for collateral

`E7A / SILVER`, `QUIET PRECISION`, `E7A / EMBER`, `ACTIVE GEOMETRY` are editorial labels, not substitutes for the official wordmark or promises of a product tier.
