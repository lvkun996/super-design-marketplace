---
name: super-ios-glass-tabbar
description: Design or refine floating mobile tab bars with a translucent iOS glass capsule, clear active state, and consistent icons. Use for tab-bar UI implementation or visual polish; not for unrelated navigation systems.
---

# Super iOS Glass Tab Bar

Use this skill when implementing or refining a mobile bottom tab bar based on the app's floating glass capsule: a translucent rounded rail, a subtle gray selected capsule, compact icons and labels, and a safe-area-aware position.

## Design recipe

- Keep destinations in equal-width slots inside one floating, fully rounded rail. For a four-item mobile bar, a useful starting point is a 68px rail, 5px inner padding, and `min(480px, calc(100% - 32px))` width. Adapt dimensions to the host app and label lengths.
- Float the rail above the device home indicator using `env(safe-area-inset-bottom)`. Add enough bottom padding to scrollable page content so the bar does not cover the final item.
- Give the rail a low-opacity cool white surface, a thin light edge, a restrained shadow, and a backdrop blur. The current reference uses `rgba(250, 250, 252, .24)` with `blur(12px) saturate(150%)`. Keep page content visible through the material; avoid raising opacity or blur until it looks like an opaque toolbar.
- Place one rounded selected-state capsule behind the tab contents. Use `rgba(224, 224, 228, .3)` without an additional backdrop blur, border, or shadow. Keep it inside the selected slot with a small inset; it must not cover neighboring labels or icons.
- Use `#1c1c1e` for inactive content and `#007aff` as the single blue accent for the selected icon and label. Maintain readable contrast over the translucent surface.
- Prefer one stable outline icon per destination. Avoid switching one icon from outline to solid on selection when the glyphs have visibly different weight or geometry. Icons should inherit `currentColor`; clear unintended filters, text shadows, backgrounds, or inherited effects that can cause dark blocks or halos.

## Implementation guidance

- First inspect the app's existing tab markup, icon source, cascade order, safe-area handling, and reduced-motion/transparency rules. Make the smallest scoped change and account for later overrides in the stylesheet.
- Keep the moving/selected capsule behind tab items, non-interactive, and within an isolated stacking context. The capsule should not intercept taps. Center it vertically with `top: 50%` and `translate3d(x, -50%, 0)`; use `calc(100% - 6px)` height. Measure the selected item for width and horizontal offset, subtract 4px from the slot width, and clamp the capsule within the rail. Recalculate after route changes and resize; enable transitions only after initial geometry is measured.
- Animate capsule position and width for 330ms with `cubic-bezier(.22, .78, .24, 1)`. Keep one moving indicator rather than separate overlapping selected backgrounds.
- Use 25px icons and 11px labels with weight 600, a 3px icon-label gap, and `4px 2px` item padding. Give tab contents a higher stacking level than the indicator.
- When diagnosing unexplained black patches, inspect the rendered icon glyph/font, computed `color`, pseudo-elements, filters/shadows, transforms, stacking order, and backdrop-filter support. Fix the responsible layer instead of hiding it with a broad black/white override.
- Preserve the app's existing navigation semantics, active-route behavior, labels, and icon family unless the user asks to change them.
- Provide an opaque `#f8f8fa` rail when backdrop filtering is unsupported. Respect `prefers-reduced-transparency` by removing backdrop filters and using an opaque `#f2f2f7` neutral rail/capsule. Respect `prefers-reduced-motion` by disabling capsule transitions.
- Verify the actual mobile-sized rendering when visual acceptance matters. Check all destinations, selected and unselected states, narrow widths, safe-area clearance, and page-content clearance. Report source-only or build checks separately from visual verification.

## Reference values from the source design

Updated from the current online-excel tab bar on 2026-10-14. These are starting values, not universal requirements:

| Part | Reference |
| --- | --- |
| Mobile rail | 68px high; 5px padding; width `min(480px, calc(100% - 32px))`; radius 999px |
| Rail material | `rgba(250, 250, 252, .24)`; `blur(12px) saturate(150%)` |
| Rail edge / shadow | `1px solid rgba(255, 255, 255, .9)`; `0 2px 12px rgba(0, 0, 0, .06), inset 0 1px 0 rgba(255, 255, 255, .65)` |
| Selected capsule | Height `calc(100% - 6px)`; measured slot width minus 4px; fully rounded |
| Selected material | `rgba(224, 224, 228, .3)`; no separate blur, border, or shadow |
| Inactive / active color | `#1c1c1e` / `#007aff` |
| Icon / label | 25px / 11px; label weight 600; gap 3px |
| Indicator transition | 330ms; `cubic-bezier(.22, .78, .24, 1)`; position and width |
| Bottom offset | `max(12px, calc(env(safe-area-inset-bottom, 0px) + 10px))` |

Use the current product's design system and user-provided reference when they differ from these values.
