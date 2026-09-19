# Super UI Design Visual System

Use this reference for product interfaces such as productivity tools, editors, dashboards, settings, desktop applications, and content-heavy web apps. Adapt values to an existing system when necessary, but preserve the relationships between density, shape, and elevation.

## Contents

- Design character
- Spacing and layout
- Typography
- Corner radius
- Borders and shadows
- Color and surfaces
- Components and states
- Motion and responsive behavior
- Acceptance checklist

## Design character

Build around five qualities:

- **Content first:** Give documents, data, editors, and canvases the largest uninterrupted surfaces.
- **Quiet density:** Keep navigation and controls compact without making them cramped.
- **Soft precision:** Use hairline boundaries, restrained neutrals, exact alignment, and clear focus rings.
- **Layered restraint:** Keep persistent structure flat; allow temporary UI to float.
- **Selective expression:** Reserve playful type and atmospheric color for empty or inspirational moments.

The visual signature is closer to a focused desktop workspace than a card-based web dashboard.

## Spacing and layout

Use a 4 px base rhythm. Prefer this compact scale:

| Token | Value | Typical use |
| --- | ---: | --- |
| `space-0.5` | 2 px | Optical alignment, tiny internal offsets |
| `space-1` | 4 px | Dense icon and menu gaps |
| `space-1.5` | 6 px | Compact controls and tab content |
| `space-2` | 8 px | Related controls, list rows |
| `space-3` | 12 px | Control groups, compact panel padding |
| `space-4` | 16 px | Standard row and section spacing |
| `space-5` | 20 px | Content padding and larger gaps |
| `space-6` | 24 px | Dialog padding, section separation |
| `space-8` | 32 px | Reading surfaces and spacious sections |
| `space-12` | 48 px | Empty states and immersive layouts |
| `space-16` | 64 px | Exceptional display spacing only |

Density anchors:

- Use 28–32 px heights for compact icon buttons, tabs, menus, and desktop chrome.
- Use 34–40 px heights for comfortable inputs and primary controls.
- Keep persistent side navigation around 220–260 px unless content requires more.
- Keep reading content around 760–840 px wide; let editors, tables, and canvases use the full work pane.
- Use roughly 20 px horizontal content padding at narrow desktop widths.
- Join structural regions edge to edge. Do not add outer padding merely to imitate a dashboard.

## Typography

Use the native system stack by default:

```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
  "Microsoft YaHei", sans-serif;
```

Use a compact hierarchy:

| Role | Size / line height | Weight |
| --- | --- | ---: |
| Tertiary metadata, shortcuts | 11 px / 16 px | 400–500 |
| Tabs, menus, helper text | 12 px / 18 px | 400–500 |
| Default UI and editor text | 13 px / 20 px | 400 |
| Titles and comfortable controls | 14 px / 22 px | 500–600 |
| Section heading | 16 px / 24 px | 600 |
| Dialog or page title | 19–20 px / 28 px | 700 |
| Reading body | 15–16 px / 1.72–1.78 | 400 |
| Expressive empty-state title | 40–64 px | 600–700 |

Avoid excessive bolding. Use `JetBrains Mono`, `SFMono-Regular`, Consolas, or an equivalent only for code, shortcuts, and numeric displays. Keep expressive or handwritten fonts out of routine navigation, settings, and editing UI.

## Corner radius

Use a role-based radius ladder:

| Token | Value | Use |
| --- | ---: | --- |
| `radius-structural` | 0 | App shells, split panes, title bars, full-bleed surfaces |
| `radius-control` | 6 px | Dense buttons, tabs, list items, inline code |
| `radius-input` | 8 px | Inputs, search results, feedback chips |
| `radius-popover` | 12 px | Menus, command bars, contextual floating surfaces |
| `radius-dialog` | 14 px | Dialogs and prominent temporary layers |
| `radius-round` | 999 px | Pills, dots, handles, circular affordances only |

Do not use large rounded cards as the default container. A larger radius should correspond to a more temporary or elevated layer.

## Borders and shadows

Use 1 px low-contrast borders and dividers. Prefer one divider plus spacing over boxing every group. Use a 2 px outline or 2–3 px translucent ring for focus and selection.

| Elevation | Suggested shadow | Use |
| --- | --- | --- |
| Flat | none | App shell, panes, content, navigation |
| Selected | `0 1px 4px rgb(15 23 42 / 8%)` | Active row or compact selected item |
| Floating | `0 8px 24px rgb(15 23 42 / 14%)` | Menu, tooltip, small indicator |
| Overlay | `0 16px 40px rgb(15 23 42 / 18%)` | Dialog, command bar, global search |

In dark mode, reduce visible shadow reliance and strengthen separation with subtle borders and surface contrast. Shadows communicate actual elevation; they are not rectangle decoration.

## Color and surfaces

Use colors by semantic role. Reuse a project's established palette when present.

### Light direction

- Main content: white or near-white.
- Navigation: quiet warm/cool gray around `#F3F3F3`.
- Alternate surface: `#FAFBFC` to `#FBFCFE`.
- Primary text: near-black around `#1F2329`.
- Secondary text: slate gray around `#596473` to `#7A8596`.
- Divider: pale blue-gray around `#E5E8EF`.
- Interaction blue: around `#1677FF`.
- Destructive: restrained red around `#D92D20`.

### Dark direction

- Main content: deep navy around `#101722` to `#111821`, not pure black.
- Navigation: a subtly differentiated deep navy or moss-toned neutral.
- Raised surface: around `#141D28` to `#182230`.
- Primary text: around `#E8EEF7`.
- Secondary text: around `#94A1B3` to `#A9B6C8`.
- Divider: around `#263344`.
- Interaction blue: brighter, around `#72ADFF`.

Use blue for interaction and state. Avoid broad saturated fills. Persistent gradients are generally disallowed; an extremely subtle navigation treatment or expressive empty state is the exception.

## Components and states

- Keep top navigation rectangular and edge-aligned. Indicate active state with subtle lightening, a narrow marker, or a status dot rather than a raised capsule.
- Keep sidebar and list labels on one line with ellipsis. Reveal secondary controls on hover, selection, or dirty state when appropriate.
- Keep title rows compact: primary action, title, then overflow actions.
- Keep settings rows flat with a clear label, muted support text, right-aligned control, and hairline separators.
- Use dialogs with 22–24 px padding, `radius-dialog`, a soft overlay shadow, and restrained backdrop blur.
- Use dropdowns with 5–6 px outer padding, 8 px item radius, 12–13 px type, and 28–32 px rows.
- Float global search near the top center and keep it under roughly 640 px wide.
- Use cards only when the content is genuinely a discrete object, not simply to group every section.
- Design hover, pressed, focus-visible, selected, disabled, loading, empty, error, overflow, and dragging states where applicable.

## Motion and responsive behavior

- Use 120–160 ms `ease` or `ease-out` transitions for hover and focus feedback.
- A compact pressed control may scale to `0.98`.
- Dragged objects may use about `0.42` opacity and `0.97` scale.
- Animate feedback, not decorative layout movement.
- Remove nonessential animation under `prefers-reduced-motion: reduce`.
- Replace translucent layers with opaque theme-matched fills under reduced-transparency preferences.
- Below roughly 980 px, stack split panes when needed, collapse multi-column settings, and reduce display type while retaining at least 20 px content padding.
- Preserve desktop density; do not enlarge every control to mimic a mobile interface.

## Acceptance checklist

- Persistent chrome is compact; primary content is spacious and flat.
- Spacing follows the 4 px rhythm without arbitrary one-off values.
- Type remains compact, readable, and restrained.
- Radius increases with elevation instead of appearing randomly.
- Only floating surfaces receive meaningful shadows.
- Active and focused items are clear without becoming loud.
- Light and dark themes preserve the same hierarchy.
- Long Chinese and English labels truncate or wrap intentionally.
- Keyboard focus, contrast, reduced motion, and reduced transparency remain usable.
- The target viewport has no clipped primary actions or unintended horizontal overflow.
