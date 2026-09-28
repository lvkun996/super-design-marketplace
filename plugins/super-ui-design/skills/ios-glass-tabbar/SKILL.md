---
name: ios-glass-tabbar
description: Design or refine floating mobile tab bars with a translucent iOS glass capsule, clear active state, and consistent icons. Use for tab-bar UI implementation or visual polish; not for unrelated navigation systems.
---

# iOS Glass Tab Bar

Use this skill when implementing or refining a mobile bottom tab bar based on the app's floating glass capsule: a translucent rounded rail, a softly frosted selected capsule, compact icons and labels, and a safe-area-aware position.

## Design recipe

- Keep destinations in equal-width slots inside one floating, fully rounded rail. For a four-item mobile bar, a useful starting point is a 54px rail, 5px inner padding, and a width capped around 480px while keeping 12–28px side margins. Adapt dimensions to the host app and label lengths.
- Float the rail above the device home indicator using `env(safe-area-inset-bottom)`. Add enough bottom padding to scrollable page content so the bar does not cover the final item.
- Give the rail a low-opacity cool white surface, a thin light edge, a restrained shadow, and a backdrop blur. A starting point is white/gray at roughly `.5` alpha with `blur(22px) saturate(135%)`.
- Place one rounded selected-state capsule behind the tab contents. Use a translucent white fill (about `.66` alpha), a stronger blur (about `18px`), a subtle highlight and shadow, and a fine light border. Keep it inside the selected slot with a small inset; it must not cover neighboring labels or icons.
- Use dark slate for inactive content and a single blue accent for the selected icon and label. Maintain readable contrast over the translucent surface.
- Prefer one stable outline icon per destination. Avoid switching one icon from outline to solid on selection when the glyphs have visibly different weight or geometry. Icons should inherit `currentColor`; clear unintended filters, text shadows, backgrounds, or inherited effects that can cause dark blocks or halos.

## Implementation guidance

- First inspect the app's existing tab markup, icon source, cascade order, safe-area handling, and reduced-motion/transparency rules. Make the smallest scoped change and account for later overrides in the stylesheet.
- Keep the moving/selected capsule behind tab items, non-interactive, and within an isolated stacking context. The capsule should not intercept taps.
- When diagnosing unexplained black patches, inspect the rendered icon glyph/font, computed `color`, pseudo-elements, filters/shadows, transforms, stacking order, and backdrop-filter support. Fix the responsible layer instead of hiding it with a broad black/white override.
- Preserve the app's existing navigation semantics, active-route behavior, labels, and icon family unless the user asks to change them.
- Respect `prefers-reduced-transparency` by removing backdrop filters and using an opaque neutral rail/capsule. Respect `prefers-reduced-motion` by disabling capsule transitions.
- Verify the actual mobile-sized rendering when visual acceptance matters. Check all destinations, selected and unselected states, narrow widths, safe-area clearance, and page-content clearance. Report source-only or build checks separately from visual verification.

## Reference values from the source design

These are starting values, not universal requirements:

| Part | Reference |
| --- | --- |
| Mobile rail | 54px high; 5px padding; fully rounded; translucent light gray |
| Rail material | `rgba(246, 246, 246, .54)`; `blur(22px) saturate(135%)` |
| Selected capsule | 2px taller than rail content; inset 4px; fully rounded |
| Selected material | `rgba(255, 255, 255, .66)`; `blur(18px) saturate(165%)` |
| Inactive / active color | `#1c2430` / `#0752c7` |
| Bottom offset | `max(12px, calc(env(safe-area-inset-bottom, 0px) + 10px))` |

Use the current product's design system and user-provided reference when they differ from these values.
