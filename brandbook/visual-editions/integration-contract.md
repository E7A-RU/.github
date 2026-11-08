# E7A Visual Editions — Integration Contract (proposal)

**Status:** design/architecture contract for future adoption; **not an existing Core API or implemented feature**. Owner direction: edition selection must be a first-class *optional decorative surface* alongside the existing Core Dark/Light theme, with no disruption to existing products. See [Overview](README.md), [Asset Governance](asset-governance.md), [Silver](silver.md), [Ember](ember.md), [Issue #21](https://github.com/E7A-RU/.github/issues/21).

## Verified current technical baseline

As inspected on 2026-11-08 in `E7A-RU/core`:
- `packages/ui/styles/tokens.css`: canonical default dark tokens, including `--e7a-blue` and `--e7a-balance-point`.
- `packages/ui/styles/themes.css`: **optional import after `tokens.css`**, scoped selectors `[data-e7a-theme="dark"]` / `[data-e7a-theme="light"]`. Theme colors and `color-scheme` are scoped. There is no JS ThemeProvider; products choose and persist their own preference.
- `packages/ui/styles/components.css`: reusable brand/component styles.
- `packages/ui/package.json`: `@e7a-ru/ui@0.3.0` exports `tokens.css`, `themes.css`, `components.css`, but **does not export an edition stylesheet or API**.

Consequently, **never** add `silver` or `ember` as values of `data-e7a-theme` or claim `@e7a-ru/ui/editions.css` exists today. An edition is not a semantic UI theme.

## Proposed two-axis selection

| Dimension | Proposed values | Owner | Current availability |
| --- | --- | --- | --- |
| `theme` | `dark`, `light` | Core semantic styles; product preference owner | Implemented in Core 0.3.0 |
| `edition` | `none`, `silver`, `ember` | Brandbook registry; future optional shared Core styles; product preference owner | **Specification only** |

Defaults: `theme=dark` if the product has no other documented default; `edition=none`. Products already supporting a user-selected Light theme **must keep that preference**. An edition can be chosen independently but MUST NOT silently switch UI theme or change meaningful/status colors. For first implementation, edition backgrounds are approved for *dark* surfaces only; if the active surface is Light, display the ordinary Light background and retain the stored edition preference, so returning to Dark can restore the decorative treatment. A Light-compatible edition rendering requires a separate verified design.

## Element/scope contract (future opt-in)

A product may mark an **owned root or isolated preview container** with two independent attributes:

```html
<!-- Illustrative integration target; edition selectors are NOT shipped yet. -->
<section data-e7a-theme="dark" data-e7a-edition="silver">
  <div class="e7a-edition-backdrop" aria-hidden="true"></div>
  <main class="product-content">Accessible product content</main>
</section>
```

Do not set `data-e7a-edition` on `html` globally unless product ownership and isolation are proven. A missing attribute or `none` must be visually equivalent to current default behavior. When editions are not packaged, the extra attribute has no visual side effects.

**CSS import order for a future vetted version:**

```css
@import "@e7a-ru/ui/tokens.css";
@import "@e7a-ru/ui/themes.css";      /* currently available */
@import "@e7a-ru/ui/components.css";  /* currently available */
/* Future: optional edition stylesheet only AFTER a separately released Core version
   actually exports it; do NOT add a non-existent import now. */
/* @import "@e7a-ru/ui/editions.css"; */
```

The edition stylesheet, if approved, must:
1. Use explicit `[data-e7a-edition="silver"]` or `"ember"` scopes **together with** `[data-e7a-theme="dark"]`; do not apply to an unmarked root, semantic form controls or interactive widgets.
2. Define ONLY `--e7a-edition-*` properties and dedicated decorative `.e7a-edition-backdrop` / surface classes; **never** assign `--e7a-bg`, `--e7a-text`, `--e7a-muted`, `--e7a-blue`, `--e7a-balance-point`, `color-scheme`, focus, disabled or semantic state colors.
3. Remain optional/tree-shakeable, without resets, fetching fonts, new JS providers or mandatory scripts; behave correctly without network for local-first products.
4. Be layered behind interactive content, with `pointer-events: none` and `aria-hidden="true"`; ensure legible text on top of the actual product background. Use a static fallback and disable decorative motion on `prefers-reduced-motion`.
5. Compose with nested Dark/Light scopes: an inner `[data-e7a-theme="light"]` must not inherit dark decorative art. **Scope the actual artwork to its own explicitly marked dark container**, never a top-level descendant rule that leaks through nested light sections. For nested dark areas, only separately opted-in surfaces render decoration.

### Reference CSS (illustrative design contract; not published implementation)

```css
/* All selectors strictly local to an opted-in dark surface. */
[data-e7a-theme="dark"][data-e7a-edition="silver"] > .e7a-edition-backdrop {
  --e7a-edition-surface: #11141b;
  --e7a-edition-light: rgba(245, 247, 250, .18);
  background:
    radial-gradient(ellipse at 78% 35%, var(--e7a-edition-light), transparent 58%),
    var(--e7a-edition-surface);
}
[data-e7a-theme="dark"][data-e7a-edition="ember"] > .e7a-edition-backdrop {
  --e7a-edition-surface: #0f1117;
  --e7a-edition-light: rgba(255, 98, 24, .24);
  background:
    linear-gradient(126deg, transparent 42%, var(--e7a-edition-light) 48%, transparent 54%),
    var(--e7a-edition-surface);
}
[data-e7a-edition] > .e7a-edition-backdrop {
  pointer-events: none;
}
/* Containing/layout positioning are product-owned. No HTML/body reset. */
```

The brand mark must be rendered **separately** using immutable official Core assets; do not implement it in gradients or generated pseudo-letterforms. If real HTML uses nested edition elements rather than direct children, adapt the selectors only with consumer QA to avoid scope leaks.

## Preference contract and compatibility

- Products manage preference state; **Core does not own storage, user accounts, Identity or tenant policy**.
- Preferred logical schema for *new* product integrations: `{ theme: "dark" | "light", edition: "none" | "silver" | "ember" }`. This is an example DTO, **not a shared runtime protocol**.
- Only add `edition` to an existing settings schema using a product-owned versioned migration that preserves all prior keys/values and supports rollback. Invalid/unknown edition IDs must resolve visually to `none` without deleting or rewriting stored preferences.
- Default `none`; explicit opt-in selector; one-action reset to `none`; don't show unavailable editions as functional choices.
- Server rendering/hydration: avoid a flash of incorrect edition; pre-render safe `none` or use the product's existing preference bootstrapping. No requirement to block paint on Identity/network.
- New shared API requires versioned Core package release, archive/readback verification, consumer exact-version lockfile and independent product rollout. No implicit upgrade.

## Integration acceptance matrix (future implementation)

| Scenario | Required observable result |
| --- | --- |
| No edition stylesheet or attribute | Identical baseline Core Dark/Light display |
| Dark + none | Unchanged Dark UI |
| Light + none | Unchanged Light UI |
| Dark + Silver | Only approved decorative surface appears; blue mark and semantic colors unchanged |
| Dark + Ember | Only approved decorative surface appears; warm light not used for status |
| Light + Silver / Ember | Ordinary accessible Light surface, saved edition retained but decoration suppressed |
| Nested Light in Dark edition | No dark decoration leak into Light scope |
| Nested Dark without explicit edition | No accidental edition inheritance |
| Invalid / future edition ID | Safe none rendering; stored unknown value not destructively erased |
| Keyboard, screen readers, reduced motion | Accessible focus/actions; decorative art non-interactive/static fallback |
| Local-first/offline | Full baseline UI without fetching edition assets |
| Upgrade/rollback | No preference loss; edition removal reverts to existing UI |

## Adoption sequence / gates

**This documentation PR:** Brandbook design and architecture review only; no runtime, Core implementation, new CSS export or product release.

**Separate Core implementation PR only when scheduled:** exact package API choice, optional CSS export, contract and snapshot tests, dark/light nesting cases, static/reduced-motion coverage, tagged immutable release and verified registry readback.

**Separate pilot (Admin Template synthetic preview is a candidate):** pin vetted Core version, test theme/edition matrix, preference migration, keyboard/a11y, mobile/desktop, rollback; one product owner signs off.

**Later consumers:** optional independent opt-ins; Website previews require editorial approval; Reader privacy/offline and saved preferences cannot be regressed. No automatic rollout by Automation.

## Merge-readiness interpretation

This document can be merged as a **proposal** without creating new active UI features. Do not mark the architectural connection as `IMPLEMENTED` or `TESTED` solely based on this documentation. If the requested acceptance standard is **usable themes in products today**, this PR alone is insufficient: Core and consumer implementation/testing must happen as separately authorized development.
