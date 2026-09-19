---
name: super-ui-design
description: Design, implement, refine, or review web and desktop product interfaces in Lvkun's quiet, content-first visual style. Use for product screens, dashboards, editors, navigation, settings, panels, dialogs, search, empty states, responsive layouts, design systems, CSS, and frontend components when the user asks for Lvkun's style, a personal/default UI style, or visual consistency across projects. Do not use for business logic, backend-only work, release work, or marketing pages that explicitly require a different brand direction.
---

# Super UI Design

Create calm, compact, content-first product interfaces. Keep persistent structure flat and quiet; reserve stronger radius, blur, color, and shadow for temporary or interactive layers.

Before making visual decisions, read [references/visual-system.md](references/visual-system.md). Treat its values as a coherent token system, not a menu of unrelated effects.

## Apply the style

1. Inspect the existing product, framework, components, and design tokens before changing the UI.
2. Preserve product behavior, information architecture, copy, and data flow unless the user requests changes.
3. Establish hierarchy with spacing, type, and hairline dividers before adding containers.
4. Let the main content occupy a broad, uninterrupted surface. Avoid wrapping the workspace in decorative cards.
5. Keep navigation and controls compact. Make interactive states quiet at rest and unmistakable on hover, focus, selection, or drag.
6. Use semantic tokens and reuse existing components. Reconcile existing values toward the reference rather than introducing near-duplicates.
7. Pair light-mode decisions with deliberate dark-mode values when the product supports themes.
8. Implement responsive behavior without turning desktop chrome into oversized mobile UI.

## Make contextual decisions

- Use flat, square structural regions for app shells, workspaces, split panes, title bars, and persistent navigation.
- Use small radii for dense controls and medium radii for inputs or contextual surfaces.
- Use larger radii, blur, and visible shadows only for dialogs, menus, command bars, global search, and other floating layers.
- Use blue for focus, links, selection, drag/drop, and actionable state—not as broad decorative fill.
- Allow expressive type, atmospheric color, or subtle grids only in intentional empty, onboarding, or inspiration states.
- If the project already has a strong brand system, preserve its identity while applying this skill's density, hierarchy, elevation, and interaction discipline.

## Avoid generic output

- Do not default to card grids, statistic tiles, thick borders, oversized headings, excessive pills, or generic admin-dashboard composition.
- Do not add gradients, glass effects, or shadows to persistent content surfaces.
- Do not apply a different radius or spacing value to every component.
- Do not use decorative color where spacing, typography, or a divider can provide hierarchy.
- Do not invent a new component when an existing project component can express the same role.

## Validate the result

Review the implemented interface at its target viewport and at a narrower supported width. Check normal, hover, pressed, focus-visible, selected, disabled, loading, empty, error, overflow, and dark states where relevant. Verify readable contrast, keyboard focus, reduced motion, long Chinese and English labels, and the absence of unintended horizontal overflow.

The finished interface should feel quiet when idle, precise when scanned, and clear when interactive.
