# E7A Silver — Quiet Precision

**Edition ID:** `silver` · **status:** exploratory specification / no released asset or UI theme · [overview](README.md) · [asset rules](asset-governance.md).

## Creative brief

**Proposition:** technological competence expressed through restraint, careful geometry and precise light. Emotional targets: calm, trust, control, maturity, attention to detail, not spectacle. Use a light touch: the brand must remain readable even if all reflective material effects are removed.

### Visual grammar

| Dimension | Guidance |
| --- | --- |
| Substrate | Deep graphite and satin gunmetal; dry matte surfaces with mild micrograin |
| Main forms | Long arcs, broad sweeping planes, measured diagonals, large quiet fields |
| Lighting | A single soft white/silver rim light, controlled diffusion, never a chrome mirror wall |
| Depth | 2–3 planes normally sufficient; low clutter; low frequency texture |
| Technical detail | Thin registration guides, small axis markers, sparse coordinate ticks only when meaningful |
| Spatial rhythm | Broad negative space; one principal curve and subordinate tracing lines |
| Logo | Exact Core wordmark or E7AMark in unmodified proportions; canonical blue point remains blue |
| Type | Manrope for body and titles, JetBrains Mono for short metadata, restrained tracking |

### Proposed extension palette (NOT canonical UI tokens)

These **draft sRGB design-study values** are deliberately namespaced. Final surface colors and print conversions require mockup/contrast review. Do not copy them to Core without a separate review.

| Token candidate | Study value | Role |
| --- | --- | --- |
| `--e7a-edition-silver-surface-1` | `#11141B` | Primary dark decorative plane |
| `--e7a-edition-silver-surface-2` | `#1A1F28` | Receding plane |
| `--e7a-edition-silver-highlight` | `rgba(245,247,250,.72)` | Edge reflection |
| `--e7a-edition-silver-edge` | `rgba(255,255,255,.18)` | Hairline geometry |

**Immutable identity contrast:** official background `#0B0D12`, official text `#F5F7FA`, official blue `#3B82F6`. Silver tones may depict *light* but do not turn the logo node silver. Text, focus and feedback colors come from the UI theme.

## Composition rules

1. Establish a single optical focal point outside the phone clock/widget zone or key product UI content.
2. Keep generous negative space (conceptual target: about one third of frame, not a universal numeric requirement).
3. Use one principal sweeping contour; secondary lines carry subtle structural orientation, not ornament.
4. Allow highlights to taper naturally and prefer low-brightness large areas on OLED devices.
5. Put the official mark on a calm field with verified clear space; never make it part of the texture.

**Wallpaper studies:** separate lock/home variants; target iPhone 16 Pro native portrait `1206×2622` as one export preset, plus master larger than target and desktop variants. Test actual lock-screen safe area instead of assuming universal pixel coordinates; cropping and depth effects can change positioning. Preview in native device settings before approval.

**Responsive:** hero can crop from portrait to landscape, but the mark must remain intact. No critical text should sit at extreme crop edges. Create optical rather than mechanical centering as needed.

## Motion (future optional)

Slow drifting reflected light or line reveal, subtle depth shift; avoid autoplay if it distracts. Respect `prefers-reduced-motion` and provide a fully static equivalent. No decorative animation in core reading/task interaction paths.

## Recommended applications

Wallpapers, quiet brand posters, executive editorial materials, device sleeve, notebook and minimal hoodie. A product interface should use Silver only as a user-selected background/surface treatment; default controls, elevation and semantic status remain governed by Dark/Light.

## Do / don't

- **Do:** quiet matte graphite, measured curves, authentic official Core mark, tiny blue brand node.
- **Do:** preserve content legibility and optically stable composition under cropping.
- **Don't:** rebuild the E7A wordmark from metallic strokes or turn it into a chrome sculpt.
- **Don't:** use heavy noise, generic sci-fi circuit motifs, full glassmorphism, counterfeit 3D badges or unreadable thin text.
- **Don't:** market a `Silver` paid tier merely because a surface is silver.

## Mini acceptance checklist

Exact mark source SHA; licensed/provenance-checked files; contrast/focus and image crop review; mobile + desktop previews; OLED/light reflection check; reduced-motion review if animated; owner/design sign-off before public distribution; print samples and applicable brand/legal review before merchandise.
