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
10. When done, report: variables created, values still hardcoded, anything skipped and why. Then tick the component in this checklist.

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

- [ ] Radio Group
  > Recreate `radio-group.tsx` following the rules in `designs/components-checklist.md`. Include the group and the item, unchecked, checked, focus and disabled states.

- [ ] Switch
  > Recreate `switch.tsx` following the rules in `designs/components-checklist.md`. Include off, on, focus and disabled states, and every size.

- [ ] Select
  > Recreate `select.tsx` following the rules in `designs/components-checklist.md`. Include the trigger (placeholder, filled, focus, disabled, invalid), the open content with items, a selected item, a group label and a separator.

- [ ] Field
  > Recreate `field.tsx` following the rules in `designs/components-checklist.md`. Build it from Label, Input, Textarea, Checkbox and Switch instances. Include description and error message, and vertical and horizontal orientation.

- [ ] Badge
  > Recreate `badge.tsx` following the rules in `designs/components-checklist.md`. Include every variant.

- [ ] Separator
  > Recreate `separator.tsx` following the rules in `designs/components-checklist.md`. Include horizontal and vertical.

- [ ] Kbd
  > Recreate `kbd.tsx` following the rules in `designs/components-checklist.md`. Include a single key and a key group (e.g. Cmd + K).

- [ ] Tooltip
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
