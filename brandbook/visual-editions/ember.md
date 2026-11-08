# E7A Ember — Active Geometry

**Edition ID:** `ember` · **status:** exploratory specification / no released asset or UI theme · [overview](README.md) · [asset rules](asset-governance.md).

## Creative brief

**Proposition:** energy contained by engineering discipline. Ember communicates deliberate activation, structural tension, movement and focus without turning E7A into a gaming/cyberpunk identity. Emotional targets: decisive, modern, purposeful, high-agency, controlled power — never aggressive or chaotic.

### Visual grammar

| Dimension | Guidance |
| --- | --- |
| Substrate | Dark graphite, angular gunmetal, deep shadow; restrained textured material |
| Main forms | Faceted architectural planes, angular cuts, intersecting diagonals, long coherent seams |
| Lighting | One controlled warm line or recessed seam lighting, local soft spill, no full-frame flame |
| Depth | Distinct foreground/background planes; readable hierarchy; no gratuitous exploded geometry |
| Technical detail | Sparse grid/coordinate marks that emphasize orientation, not surveillance/dashboard decoration |
| Spatial rhythm | Strong directional axis plus a reserved quiet field for brand/content |
| Logo | Exact Core wordmark or E7AMark; retain canonical **blue** point |
| Type | Manrope for editorial text; JetBrains Mono for small engineering labels |

### Proposed extension palette (NOT canonical UI tokens)

Study-only values subject to design and accessibility QA:

| Token candidate | Study value | Role |
| --- | --- | --- |
| `--e7a-edition-ember-surface-1` | `#0F1117` | Dark graphic ground |
| `--e7a-edition-ember-surface-2` | `#171B24` | Structural plane |
| `--e7a-edition-ember-glow` | `rgba(255,98,24,.90)` | Non-semantic seam light |
| `--e7a-edition-ember-glow-soft` | `rgba(255,98,24,.24)` | Local spill only |

**Two accent roles, never conflated:** **orange = atmospheric light; blue = brand identity.** The true E7A node and core tokens remain `#3B82F6`. Orange may never replace warning/error/focus/success color or brand balance-point. Use warm glow sparingly to retain the identity hierarchy.

## Composition rules

1. One primary tension line/cut and one focal glow, not many competing ignition sites.
2. Diagonals must obey an intelligible architectural construction, not random broken polygons.
3. Leave a quiet field for the official mark and captions; local light cannot overpower them.
4. Light must emanate from plausible geometry and fade within adjacent surfaces.
5. Prefer 2–4 strong tonal masses over fine texture and dense grids.
6. Visual complexity must collapse gracefully for mobile crop, lock screen and reduced-motion variants.

**Wallpaper studies:** produce dedicated lock-screen and home-screen compositions and a larger master; iPhone 16 Pro portrait export candidate `1206×2622` with device-preview QA for Dynamic Island, clock, widgets, lock-screen depth placement and icon contrast. No hardcoded safe zone is normative until tested on the intended OS/layout.

**Responsive:** keep the primary seam off critical content/CTAs, retain a logo-safe quiet area, and check the crop on small Android, iPhone and desktop. Contrast must remain valid even with glow disabled.

## Motion (future optional)

Very short directional reveal of the illuminated seam, restrained focused pulse only when explicitly desirable. No constant strobes, flashing, aggressive parallax or UI alert mimicry. Static fallback and `prefers-reduced-motion` are mandatory for any actual implementation.

## Recommended applications

Product-announcement artwork, launch posters, wallpapers, stage/opening imagery, editorial campaign and limited experimental merchandise studies. Admin Template may be a future *isolated synthetic preview*; never deploy a new look to all products without their own approval.

## Do / don't

- **Do:** contained warm seam lighting, controlled facets, legible official brand sign.
- **Do:** use orange only as material light and preserve blue brand node.
- **Don't:** recolor the seven/node to red/orange or substitute an AI-generated pseudo-logo.
- **Don't:** make the interface resemble critical alerts, load failures, thermal threats or security incidents.
- **Don't:** use flames, exaggerated lens flares, continuous pulsating lights, RGB cyberpunk or aggressive gaming textures.
- **Don't:** claim the edition has been released or is a separate paid tier.

## Mini acceptance checklist

Exact Core logo/mark SHA; evidence of source asset rights; contrast/semantic differentiation checks; glare and crop verification on actual devices; static/reduced-motion version; snapshot and performance QA for any optional UI use; explicit product owner and brand approval; sample and legal review for physical goods.
