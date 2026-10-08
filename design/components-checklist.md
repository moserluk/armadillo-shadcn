# Component library checklist

Source components: `apps/v4/registry/new-york-v4/ui/`
Target: the library `.pen` file
Product: desktop app

---

## Rules for the agent (apply to every component)

1. Read the component's `.tsx` file before creating anything.
2. Recreate it as a reusable component. Include every variant and size from its `cva` config, and every subpart it exports.
3. Show these states where the component has them: default, hover, focus-visible, disabled, invalid/error, open/checked/selected.
4. Colors: bind only to `Theme` axis variables. Never hardcode a color. Never bind a component directly to a primitive color.
5. Radius, spacing, font size, weight and line height: bind to `Primitives` axis variables. Hardcode only when no primitive matches, and list every case.
6. Opacity and dark-mode classes (`x/NN`, `dark:...`): before building, list them. Reuse existing variables where they match. Propose missing ones with name, light and dark value:
   - same class in both modes -> `token-NN` (e.g. `primary-90`), referencing an `alpha/...` primitive
   - different class per mode -> `component-part` (e.g. `button-outline-bg`)
   Wait for approval before creating any variable.
7. If the component uses another component that already exists in the library (Button, Input, Separator...), use an instance of it. Don't redraw it.
8. Place the component with all variants and states in a light frame and a dark preview frame (`{Theme: "dark"}`).
9. Don't change any other component or existing variable.
10. When done, report: variables created, values still hardcoded, anything skipped and why. Then tick the component in this checklist, add a row to **Design progress** below, and add anything deferred to **Open follow-ups**.

---

## Design progress

Components finished in `design/armadillo.pen`. Frames sit in one row from left to right; each dark preview sits under its light frame.

| Component | Light / dark frame | Components | Variables added | Notes |
|---|---|---|---|---|
| Button | `usjgH` / `aMfzE` | `Button/Base` + 48 (6 variants × 8 sizes) | (before this log) | Hover states still missing |
| Input | `S4hHF` / `TgVtn` | `Input/Base` + default, focus, disabled, invalid, invalid-focus | (before this log) | Hover state still missing |
| Card | `N3KarR` / `TLv9U` | `Card` + Title, Description, Header, Content, Footer | (before this log) | |
| Label | `pqdH6` / `z705e1` | `Label/Base` + default, disabled | none | Text 14 / 500 / lh 1 hardcoded (no type primitives) |
| Textarea | `MznRt` / `P74Uy` | `Textarea/Base` + default, focus, disabled, invalid, invalid-focus | none (reuses Input's) | Fixed 64 px height (`min-h-16`); `rounded-md` drawn as `radius/lg` like Input |
| Checkbox | `AKmhn` / `jEv8R` | `Checkbox/Base` + 10 (unchecked/checked × 5 states) | none | Checked keeps `primary` border when invalid (Tailwind order: `data-*` > `aria-*`), unverified against the running app |
| Radio Group | `NFIMo` / `ZiaDY` | `RadioGroup/Item/Base` + 10, `RadioGroup` | none | Indicator drawn as an 8 px ellipse instead of lucide `CircleIcon` |
| Switch | `AfDUZ` / `oyO2V` | `Switch/Base` + 12 (2 sizes × off/on × 3 states) | `alpha/white-12`, `switch-track-off`, `switch-thumb-off`, `switch-thumb-on` | No invalid state in source |
| Badge | `N7pLyj` / `JgeHn` | `Badge/Base` + 18 (6 variants × default/focus/invalid) | `destructive-foreground` | Reuses `button-destructive-bg` |
| Field | `wqpom` / `krV6b` | Parts: `Field/Label`, `Field/Title`, `Field/Description`, `Field/Error`, `Field/Legend/{legend,label}`, `Field/Separator` (+`/plain`). Layouts: `Field/vertical/{default,description-first,textarea,invalid,disabled}`, `Field/horizontal/{checkbox,checkbox-content,switch,invalid,disabled}`, `Field/Set`, `Field/Choice/{unchecked,checked}` | `alpha/black-5`, `alpha/neutral-200-10`, `field-choice-checked-bg` | Built from Label, Input, Textarea, Checkbox, Switch and RadioGroup item instances. Negative margins (`-mt-1`, `-mt-1.5`, `-my-2`) emulated with nested gaps or ignored; `responsive` orientation not shown (container query) |
| Kbd | `IFxxj` / `KN7ZU` | `Kbd/Base` + `Kbd/key`, `Kbd/word`, `Kbd/icon`, `Kbd/icon-text`, `Kbd/Group`, `Kbd/Group/with-plus` | none | `rounded-sm` (6 px) drawn as `radius/md`. Single keys are fixed at 20 px wide (`min-w-5`; pen.dev has no min-width). Geist has no `⌃` glyph, so Control uses `Kbd/icon` with lucide `chevron-up` |
| Tooltip | `Vb8tE` / `sCGQh` | `Tooltip/Content` (text + hidden KbdGroup), `Tooltip/{top,bottom,left,right}`, `Tooltip/with-kbd` | `alpha/white-20`, `alpha/neutral-950-10`, `kbd-tooltip-bg` | Arrow = 10 px square rotated 45° (`radius/xs`) in a clipped 14×6 / 6×14 slot, centered 2 px inside the content edge, so ~5 px shows (matches `translate-y(-50%-2px)`). Kbd in-tooltip overrides (`$kbd-tooltip-bg`, `$background` text) are set once inside `Tooltip/Content`. Trigger examples use `Button/outline/default`; `sideOffset` 0. Enter/exit animations not shown |

Still hardcoded in every component: the `shadow-xs` color `#0000000D`, disabled opacity `0.5`, font size, weight and line height (no typography primitives exist), and width/height (pen.dev can't bind them to variables).

## Open follow-ups

- [ ] Badge: link hovers (`[a&]:hover` primary/90, secondary/90, destructive/90, outline/ghost accent, link underline). Waiting on the Tier 0 hover variables.
- [ ] Button: rebind `Button/destructive/*` text and icon from hardcoded `#FFFFFF` to `$destructive-foreground`.
- [ ] Badge: consider renaming `button-destructive-bg` to `destructive-bg` now that Badge shares it.
- [ ] Field: replace the drawn `$border` lines in `Field/Separator` with Separator instances once Separator is built.
- [ ] Field: `FieldError` list mode (several errors as a bulleted `ul`) not drawn; only the single-message version exists.
- [x] Kbd: in-tooltip styling (`[[data-slot=tooltip-content]_&]`). Done in Tooltip with the approved `kbd-tooltip-bg`.

---

## Prompt pattern

Each prompt below follows this pattern. Paste it as written.

---

## Tier 0: done (recheck hover states)

- [x] Button
- [x] Input
- [x] Card
- [ ] Hover states for Button and Input

  > Add hover states to Button (all variants) and Input following the rules in `designs/components-checklist.md`. Propose the hover variables first (e.g. `primary-90`, `secondary-80`, `accent-50`, `input-50`).

---

## Tier 1: basics and forms (most used)

- [x] Label
  > Recreate `label.tsx` following the rules in `designs/components-checklist.md`. Include the disabled state.

- [x] Textarea
  > Recreate `textarea.tsx` following the rules in `designs/components-checklist.md`. Reuse the variables Input uses. Include focus, disabled and invalid states.

- [x] Checkbox
  > Recreate `checkbox.tsx` following the rules in `designs/components-checklist.md`. Include unchecked, checked, focus, disabled and invalid states.

- [x] Radio Group
  > Recreate `radio-group.tsx` following the rules in `designs/components-checklist.md`. Include the group and the item, unchecked, checked, focus and disabled states.

- [x] Switch
  > Recreate `switch.tsx` following the rules in `designs/components-checklist.md`. Include off, on, focus and disabled states, and every size.

- [ ] Select
  > Recreate `select.tsx` following the rules in `designs/components-checklist.md`. Include the trigger (placeholder, filled, focus, disabled, invalid), the open content with items, a selected item, a group label and a separator.

- [x] Field
  > Recreate `field.tsx` following the rules in `designs/components-checklist.md`. Build it from Label, Input, Textarea, Checkbox and Switch instances. Include description and error message, and vertical and horizontal orientation.

- [x] Badge
  > Recreate `badge.tsx` following the rules in `designs/components-checklist.md`. Include every variant.

- [ ] Separator
  > Recreate `separator.tsx` following the rules in `designs/components-checklist.md`. Include horizontal and vertical.

- [x] Kbd
  > Recreate `kbd.tsx` following the rules in `designs/components-checklist.md`. Include a single key and a key group (e.g. Cmd + K).

- [x] Tooltip
  > Recreate `tooltip.tsx` following the rules in `designs/components-checklist.md`. Include the content with arrow on all four sides, and a version with a Kbd instance.

- [ ] Dropdown Menu
  > Recreate `dropdown-menu.tsx` following the rules in `designs/components-checklist.md`. Include items (default, hover/focus, disabled, destructive), checkbox items, radio items, label, separator, shortcut, submenu and icons.

- [ ] Dialog
  > Recreate `dialog.tsx` following the rules in `designs/components-checklist.md`. Include the overlay, header, title, description, footer with Button instances, and the close button.

- [ ] Alert Dialog
  > Recreate `alert-dialog.tsx` following the rules in `designs/components-checklist.md`. Reuse Dialog's variables. Include header, footer with action and cancel Button instances, and every size it defines.

- [ ] Tabs
  > Recreate `tabs.tsx` following the rules in `designs/components-checklist.md`. Include the list, triggers (active, inactive, hover, disabled) and content, in every variant and orientation.

- [ ] Avatar
  > Recreate `avatar.tsx` following the rules in `designs/components-checklist.md`. Include image, fallback with initials, every size, and an avatar group if defined.

- [ ] Skeleton
  > Recreate `skeleton.tsx` following the rules in `designs/components-checklist.md`. Include line, circle and card-shaped examples.

- [ ] Spinner
  > Recreate `spinner.tsx` following the rules in `designs/components-checklist.md`. Include every size, and a Button instance in loading state.

---

## Tier 2: desktop app structure

- [ ] Popover
  > Recreate `popover.tsx` following the rules in `designs/components-checklist.md`. Include the content with header, title and description if defined.

- [ ] Command
  > Recreate `command.tsx` following the rules in `designs/components-checklist.md`. Include input, list, groups with headings, items (default, selected, disabled), shortcut, separator and empty state.

- [ ] Combobox
  > Recreate `combobox.tsx` following the rules in `designs/components-checklist.md`. Use Popover and Command instances if it's built from them. Include closed, open, selected item and empty search.

- [ ] Context Menu
  > Recreate `context-menu.tsx` following the rules in `designs/components-checklist.md`. Reuse Dropdown Menu's variables. Include items, checkbox and radio items, shortcut, separator and submenu.

- [ ] Menubar
  > Recreate `menubar.tsx` following the rules in `designs/components-checklist.md`. Include the bar with triggers (default, open), and an open menu with items, shortcuts, separator and submenu.

- [ ] Toggle
  > Recreate `toggle.tsx` following the rules in `designs/components-checklist.md`. Include every variant and size, off, on, hover, focus and disabled.

- [ ] Toggle Group
  > Recreate `toggle-group.tsx` following the rules in `designs/components-checklist.md`. Use Toggle instances. Include single and multiple selection and every variant.

- [ ] Button Group
  > Recreate `button-group.tsx` following the rules in `designs/components-checklist.md`. Use Button instances. Include horizontal, vertical, with separator and with text.

- [ ] Input Group
  > Recreate `input-group.tsx` following the rules in `designs/components-checklist.md`. Use Input and Button instances. Include start and end addons (icon, text, button) and the textarea version.

- [ ] Table
  > Recreate `table.tsx` following the rules in `designs/components-checklist.md`. Include header, rows (default, hover, selected), footer and caption. Show a row with a Checkbox and a Badge instance.

- [ ] Scroll Area
  > Recreate `scroll-area.tsx` following the rules in `designs/components-checklist.md`. Include vertical and horizontal scrollbars.

- [ ] Breadcrumb
  > Recreate `breadcrumb.tsx` following the rules in `designs/components-checklist.md`. Include link, current page, separator and ellipsis.

- [ ] Alert
  > Recreate `alert.tsx` following the rules in `designs/components-checklist.md`. Include every variant, with and without icon, title and description.

- [ ] Sonner (toast)
  > Recreate the toast from `sonner.tsx` following the rules in `designs/components-checklist.md`. Include default, success, error, warning, info, loading, and a toast with an action button.

- [ ] Progress
  > Recreate `progress.tsx` following the rules in `designs/components-checklist.md`. Include 0%, 50% and 100%.

- [ ] Slider
  > Recreate `slider.tsx` following the rules in `designs/components-checklist.md`. Include single and range, focus and disabled.

- [ ] Empty
  > Recreate `empty.tsx` following the rules in `designs/components-checklist.md`. Include media (icon), title, description and actions with Button instances.

- [ ] Item
  > Recreate `item.tsx` following the rules in `designs/components-checklist.md`. Include every variant and size, with media, content, actions and as a group.

- [ ] Hover Card
  > Recreate `hover-card.tsx` following the rules in `designs/components-checklist.md`. Show an example with an Avatar instance.

- [ ] Sheet
  > Recreate `sheet.tsx` following the rules in `designs/components-checklist.md`. Include all four sides, header, footer and the close button.

- [ ] Resizable
  > Recreate `resizable.tsx` following the rules in `designs/components-checklist.md`. Include horizontal and vertical panel groups and the handle with and without the grip.

- [ ] Sidebar (do this last in Tier 2; it uses many of the above)
  > Recreate `sidebar.tsx` following the rules in `designs/components-checklist.md`. Use existing instances (Input, Separator, Tooltip, Skeleton, Sheet...). Start with: sidebar container, header, footer, group with label, menu items (default, hover, active, disabled) with icons and badges, and a submenu. Show expanded and collapsed (icon-only). Stop after these and report before building the rest.

---

## Tier 3: less common for a desktop app

- [ ] Accordion
  > Recreate `accordion.tsx` following the rules in `designs/components-checklist.md`. Include closed, open, hover and focus.

- [ ] Calendar
  > Recreate `calendar.tsx` following the rules in `designs/components-checklist.md`. Include one month with today, selected day, range selection, outside days and disabled days.

- [ ] Pagination
  > Recreate `pagination.tsx` following the rules in `designs/components-checklist.md`. Use Button instances if it's built from them. Include previous, next, active page and ellipsis.

- [ ] Navigation Menu
  > Recreate `navigation-menu.tsx` following the rules in `designs/components-checklist.md`. Include triggers (default, open) and an open content panel with links.

- [ ] Input OTP
  > Recreate `input-otp.tsx` following the rules in `designs/components-checklist.md`. Include empty, filled, active slot, separator and invalid.

- [ ] Native Select
  > Recreate `native-select.tsx` following the rules in `designs/components-checklist.md`. Reuse Input's variables. Include placeholder, filled, focus, disabled and invalid.

- [ ] Chart
  > Recreate the chart container, tooltip and legend from `chart.tsx` following the rules in `designs/components-checklist.md`. First, propose replacements for `chart-1` to `chart-5` (they still hold the docs site's blue ramp).

- [ ] Carousel
  > Recreate `carousel.tsx` following the rules in `designs/components-checklist.md`. Include the previous and next buttons as Button instances, horizontal and vertical.

- [ ] Drawer
  > Recreate `drawer.tsx` following the rules in `designs/components-checklist.md`. Include the handle, header and footer. (Mostly a mobile pattern, so low priority on desktop.)

### Unknown origin: check first

These may be custom or newer chat components. Read each file and describe what it is before building.

- [ ] Attachment
- [ ] Bubble
- [ ] Marker
- [ ] Message
- [ ] Message Scroller

  > Read `[file].tsx` and tell me what it is, what it renders and which components it depends on. Change nothing.

---

## Skip: no visual design

- `_registry.ts`: registry config, not a component
- `aspect-ratio.tsx`: layout helper only
- `direction.tsx`: text direction provider
- `collapsible.tsx`: show/hide logic, no styling
- `form.tsx`: form logic wrapper; Field covers the visuals
