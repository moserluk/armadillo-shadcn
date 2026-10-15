# Component library checklist

Source components: `apps/v4/registry/new-york-v4/ui/`
Target: `armadillo.lib.pen` (the library). Never create or edit components in `armadillo-design.pen` or any other `.pen` file.
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

**Done: 47 / 52** (Tier 0: 3/3 · Tier 1: 18/18 · Tier 2: 17/22 · Tier 3: 9/9; "Unknown origin" and "Skip" not counted). Tier 1 complete. Next: Tier 2 (Empty, Hover Card, Sheet, Resizable, Sidebar phase 2) and the Tier 0 hover states.

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
| Dialog | `Ynpjp` / `Jo7vA` | `Dialog/Overlay`, `Dialog/Content` (+`/with-body`, `/no-close-button`), `Dialog/Header`, `Dialog/Title`, `Dialog/Description`, `Dialog/Footer` (+`/with-close`), `Dialog/Close/{default,hover,focus}` (`DvMJz`, `vcAcB`, `fJULA`; rebuilt after a reverted swap to `Button/ghost/icon-xs`) | `alpha/black-50`, `overlay` | Footer uses `Button/outline` + `Button/default` instances; body example uses `Field/vertical/default` instances. Content 512 px (`sm:max-w-lg`, desktop layout). `rounded-lg` = 10 px hardcoded (no primitive); `shadow-lg` approximated with 2 shadows `#0000001A` (no spread). Close is a 20 px box (16 px icon + 2 px ring offset) placed absolute at 478,14 = `top-4 right-4`; focus = 2 px `$ring` outer stroke. `data-[state=open]:bg-accent` on Close skipped (never applies). "Dialog open" scene shows trigger page + overlay + centered content |
| Tabs | `v30QoK` / `sNFvZ` | `Tabs/Trigger/Base` + 20 (`Tabs/Trigger/{default,line}/{horizontal,vertical}/{inactive,hover,focus,active,disabled}`), `Tabs/List/{default,line}/{horizontal,vertical}`, `Tabs/Content`, `Tabs/{default,line}/{horizontal,vertical}` | `alpha/black-60`, `tabs-trigger-fg`, `tabs-trigger-active-bg`, `tabs-trigger-active-border` | Border emulated with `spacing/px` padding (pen.dev ignores `layoutIncludeStroke`). Line indicator is an in-layout 2 px bar (3 px gap below / 2 px gap right), so it doesn't need absolute positioning. Vertical lists use a fixed width (widest trigger + padding) to mimic `w-fit` + `w-full` triggers. Horizontal triggers are `fit_content`, not `flex-1` equal widths. `outline-1 outline-ring` merged into the 1 px `ring` border + 3 px `ring-50` ring. `TabsContent` has no styling of its own, so it's drawn as a slot holding a Card instance |
| Avatar | `ySrnX` / `qAjEe` | `Avatar/Base` + 12 (`Avatar/{sm,default,lg}/{fallback,image,badge,badge-icon}`), `Avatar/GroupCount/{sm,default,lg}`, `Avatar/Group/{sm,default,lg}` | none | Sizes 24 / 32 / 40. Root = ring/badge layer (no clip); inner `Content` is the circle clip holding the `$muted` fallback + initials (14, or 12 for sm) or the image fill (Unsplash stock, sources kept in a hidden `Photo assets` frame). Badge absolute at the bottom-right corner (8 / 10 / 12 px, `$primary`, 2 px `$background` ring, 8 px lucide `check` on default/lg, hidden on sm). Group uses a −8 gap (`-space-x-2`) with 2 px `$background` outer strokes; GroupCount shows `+3` or a lucide `plus` (12 / 16 / 20) |
| Spinner | `yphYq` / `BBUHW` | `Spinner` (= size-4, 16 px lucide `loader-circle`, `$foreground`), `Spinner/size-{3,6,8}` (12 / 24 / 32), `Spinner/Button/{default,outline,secondary}` | none | No variants in source; sizes follow the `spinner-size` example (`className="size-N"`). Color is `currentColor` in code, so each instance sets the fill of its context (`$primary-foreground`, `$foreground`, `$secondary-foreground`). Loading buttons = `Button/{default,outline,secondary}/sm` instances, opacity 0.5 (`disabled`), icon slot replaced by a Spinner instance, padding `spacing/2.5` (`has-[>svg]:px-2.5`). Badge examples (`spinner-badge`) use `Spinner/size-3` inside Badge. `animate-spin` not shown (static) |
| Alert Dialog | `b44p1` / `C14ev` | `AlertDialog/Title`, `/Description` (instances of `Dialog/Title` / `Dialog/Description`), `AlertDialog/Media`, `AlertDialog/Header/{default,sm,default-media,sm-media}`, `AlertDialog/Footer/{default,sm}`, `AlertDialog/Content/{default,sm,default-media,sm-media,destructive}` | none (reuses Dialog's: `$overlay`, `$background`, `$border`, `$muted`) | Sizes default 512 (`sm:max-w-lg`) and sm 320 (`max-w-xs`); same shell as `Dialog/Content` (radius 10, `p-6`, `gap-4`, `shadow-lg`) but no close button. Header `gap-1.5`; Title line height 28/18 (no `leading-none`, unlike DialogTitle). sm centers the header and splits the footer into 2 equal buttons (`grid-cols-2`). Media = 64 px `$muted` tile, `$radius/lg`, 32 px lucide `bluetooth`; default = media left + text (`gap-x-6`), sm = stacked with `mb-2`. Cancel = `Button/outline`, Action = `Button/default` (destructive example uses `Button/destructive`). Open scene reuses `Dialog/Overlay` |
| Dropdown Menu | `ySSxb` / `yK70l` | `DropdownMenu/Item/{default,focus,disabled,destructive,destructive-focus,inset}` (icon, shortcut, chevron toggles), `DropdownMenu/CheckboxItem/{unchecked,checked,focus,disabled}`, `DropdownMenu/RadioItem/{unchecked,checked,focus,disabled}`, `DropdownMenu/Label/{default,inset}`, `DropdownMenu/Separator`, `DropdownMenu/SubTrigger/{default,open}`, `DropdownMenu/Content`, `DropdownMenu/SubContent` | `alpha/red-600-10`, `menu-item-destructive-focus-bg` | Item `px-2 py-1.5`, `rounded-sm` = `radius/md`, 16 px lucide icons in `$muted-foreground` (destructive: `$destructive`). Shortcut = 12 px, `tracking-widest` (1.2 px), `$muted-foreground`. Checkbox/Radio: `pl-8` with a 14 px indicator absolute at left-2 (16 px check / 8 px dot, `currentColor` = `$popover-foreground`). Content: 224 px (`w-56` demo), `$popover`, `radius/lg`, `shadow-md` (2 shadows, no spread); SubContent `shadow-lg`. `-mx-1` separator emulated: Content has vertical padding only, items sit in Groups with 4 px side padding, separators span full width. Scene: Open trigger (`sideOffset 4`), account menu with open submenu, checkbox and radio menus. Animations not shown |
| Context Menu | `Px2z8` / `YRhm9` | `ContextMenu/Label/{default,inset}`, `ContextMenu/SubTrigger/{default,open}`, `ContextMenu/Trigger`, `ContextMenu/Content` (w-52, 208), `ContextMenu/SubContent` (w-44, 176) | none (reuses Dropdown Menu's, incl. `$menu-item-destructive-focus-bg`) | Classes are identical to Dropdown Menu except Label adds `text-foreground` (bound to `$foreground`) and SubTrigger drops `gap-2` (gap 0). Items, checkbox/radio items, shortcut, separator and inset/disabled/destructive states are `DropdownMenu/*` instances, so the two menus stay in sync. Demo follows `context-menu-demo` (Back / Forward disabled / Reload, More Tools submenu with destructive Delete, checkbox items, radio group with inset label), opened at a cursor over the trigger area. Trigger's `border-dashed` drawn solid (pen.dev has no dashed strokes) |
| Popover | `ZVEqh` / `yfRnH` | `Popover/Content` (w-72, 288), `Popover/Content/dimensions` (w-80 demo) | none | Content `rounded-md` = `radius/lg`, `p-4`, `$popover` / `$popover-foreground`, `$border`, `shadow-md` (2 shadows). No PopoverHeader/Title/Description exist in new-york-v4, so the heading + muted text come from `popover-demo` (h4 `leading-none font-medium`, p `text-sm text-muted-foreground`). Demo form uses Label + Input (h-8) instances in a 3-col grid (label 85 px). Scene: outline trigger with content centered below, `sideOffset 4` |
| Command | `dLJ4k` / `kB2ED` | `Command/Input`, `Command/GroupHeading`, `Command/Item/{default,selected,disabled}` (instances of `DropdownMenu/Item/default`), `Command/Separator`, `Command/Empty`, `Command` (demo), `Command/empty` | none | Input row `h-9 px-3 gap-2 border-b` (bottom-only stroke), search icon `opacity-50`, placeholder `$muted-foreground`. Items reuse Dropdown item (identical classes; `data-selected` = accent) with text `$foreground` (group sets `text-foreground`). Group `p-1`, heading 12/500 muted. Separator full-width 1 px (`-mx-1`, no margin). Empty `py-6` centered. Demo root uses `rounded-lg` (10, hardcoded), border, `shadow-md`, 450 wide. `CommandDialog` (h-12 input, py-3 items, 20 px icons) not drawn |
| Menubar | `BAztb` / `rvbBc` | `Menubar/Bar`, `Menubar/Trigger/{default,open}`, `Menubar/SubTrigger/{default,open}`, `Menubar/CheckboxItem/focus`, `Menubar/RadioItem/focus`, `Menubar/Content`, `Menubar/SubContent` | none (reuses Dropdown Menu's) | Bar `h-9 gap-1 p-1 rounded-md border bg-background shadow-xs`; Trigger `px-2 py-1` 14/500, open = `$accent`. Items, label, separator, shortcut are `DropdownMenu/*` instances (identical classes). Differences: checkbox/radio items `rounded-xs` (`radius/xs`), SubTrigger has no `gap-2` and its chevron uses currentColor (`$popover-foreground`). Content placed `align start`, `alignOffset -4`, `sideOffset 8`. Demo = File menu open with Share submenu (menubar-demo) |
| Toggle | `YjBFU` / `YlnwF` | `Toggle/Base` + 30 (`Toggle/{default,outline}/{sm,default,lg}/{off,hover,on,focus,disabled}`) | none | Two-layer like Input (root = 3 px `$ring-50` focus ring, Body = fill/border). Sizes 32/36/40 with `min-w` = height (icon-only width fixed), `px-1.5/2/2.5`, `rounded-md` = `radius/lg`. Hover: default `$muted`/`$muted-foreground`, outline `$accent`/`$accent-foreground`; on = `$accent`/`$accent-foreground`; outline = `$input` border + `shadow-xs`, focus border `$ring` (default variant has no border, ring only); disabled opacity 0.5. Icon lucide `bold` 16; hidden 14/500 label shown in the "with text" row. aria-invalid not drawn |
| Toggle Group | `IuPH9` / `gfvhB` | `ToggleGroup/{default,outline}/{single,multiple,states}`, `ToggleGroup/outline/{sm,lg}`, `ToggleGroup/{default,outline}/spacing` | none | Items are `Toggle/*` instances with `px-3`, width fit (no `min-w`). spacing=0: outer corners only (`first:rounded-l-md`, `last:rounded-r-md`, via per-corner radius), outline items drop the left border except the first (per-side strokeWidth) and lose their own `shadow-xs`, which moves to the group. Single = one item on, multiple = two on; a states group shows hover / focus / disabled. spacing>0 example uses gap 4 with fully rounded items |
| Button Group | `xYjRc` / `IoAD5` | `ButtonGroup/Text`, `ButtonGroup/Separator`, `ButtonGroup/{demo,horizontal,vertical,vertical/text,with-separator,with-text}` | none | All buttons are `Button/*` instances; joins done with per-corner radius (outer ends keep `radius/full`, inner corners 0 = `rounded-l/r/t/b-none`) and per-side stroke (`border-l-0` / `border-t-0` on non-first outline items). Demo = nested groups with `gap-2` (back · Archive/Report · Snooze/⋯). ButtonGroupText = `rounded-md` (radius/lg) `border bg-muted px-4` 14/500 `shadow-xs`. Separator = 1 px `$input` line (drawn; swap for a Separator instance later). with-text example = Text + Input + Button |
| Input Group | `T2bpXn` / `QE2cC` | `InputGroup/Base` (hidden inline-start / inline-end addon slots around an Input instance), `InputGroup/{icon,text-button,check,kbd,button}`, `InputGroup/{focus,invalid,disabled}`, `InputGroup/textarea` | none | Root `rounded-md border-input shadow-xs`, `$input-bg` (dark `bg-input/30`), h-9, 384 wide (`max-w-sm`). InputGroupInput / Textarea = Input / Textarea instances with border, fill, shadow and radius removed; padding follows `pl-2` / `pr-2` next to addons. Addons `pl-3` / `pr-3`, 14/500 `$muted-foreground`; next to a button the negative `-0.45rem` margin is drawn as 5 px padding (hardcoded), next to Kbd 6 px. InputGroupButton = `Button/ghost/xs` (radius 5 = `calc(var(--radius)-5px)`, hardcoded) or `Button/*/icon-xs` (`rounded-full` per demo). Kbd example uses `Kbd/Group`. Focus = `$ring` border + 3 px `$ring-50`; invalid = `$destructive` border; disabled drawn as 0.5 opacity on the whole group. Textarea toolbar: outline +, ghost Auto, 52% used, 16 px vertical separator (drawn), disabled send |
| Combobox | `o3fUNM` / `MWUkc` | `Combobox/Input/{default,value,clear,focus}` (InputGroup instances), `Combobox/Item/{default,highlighted,selected,disabled}`, `Combobox/Label`, `Combobox/Empty`, `Combobox/Chip`, `Combobox/Chips`, `Combobox/Content`, `Combobox/Content/empty` | `alpha/black-10`, `alpha/neutral-50-10`, `foreground-10`, `input-30` | Recreates `ui/combobox.tsx` (Base UI), not the Popover + Command demo. Input = `InputGroup/Base` instance with a ghost icon-xs trigger (chevron, radius 5) or clear (×). Content: `rounded-md` (radius/lg) `$popover`, `shadow-md`, `ring-1 ring-foreground/10` = 1 px outer `$foreground-10` stroke (no border), width = input + 28 (`min-w calc(anchor + spacing-7)`), `sideOffset 6`, list `p-1`. Item `pl-2 pr-8`, highlighted = `$accent`, selected check at right-2 (kept in layout, opacity 0 when unselected). Label 12 muted, Empty `py-2` centered muted. Chips: `$input-bg` field with 22 px `$muted` chips (12/500) + 50% ghost icon-xs remove (overflows the chip by 1 px, as in code). `input-30` (popup search input) created but unused: no popup-input example drawn |
| Table | `N5UEbI` / `o7MPfD` | `Table/Row/{default,hover,selected}`, `Table` (header + body + footer + caption) | `alpha/neutral-100-50`, `alpha/neutral-800-50`, `muted-50` | Head `h-10 px-2` 14/500 `$foreground`; Cell `p-2` 14; rows `border-b $border`, last body row no border; hover = `$muted-50` (`bg-muted/50`), selected = `$muted`. Footer `border-t` + `$muted-50`, 500 weight. Caption `mt-4` (gap 16) muted, centered (caption-bottom). Selection column: Checkbox instances (`pr-0`, `translate-y-[2px]` = 10/6 vertical padding), selected row = checked; Status column = Badge instances (secondary Paid, outline Pending, destructive Unpaid). Demo data = table-demo (5 rows), 600 wide with fixed column widths |
| Scroll Area | `JMbTC` / `fQpYJ` | `ScrollArea/Scrollbar/{vertical,horizontal}`, `ScrollArea/vertical` (h-72 w-48), `ScrollArea/horizontal` (w-96), `ScrollArea/vertical/focus` | none | Scrollbar 10 px (`w-2.5` / `h-2.5`) with `p-px` + 1 px transparent leading border (padding 2 on that side), thumb `rounded-full $border`; thumb length = viewport / content ratio, shown scrolled to start. Viewports clip (`overflow-hidden` on Root), `rounded-md` border. Vertical = scroll-area-demo Tags list (separators drawn as 1 px `$border`, `my-2`); horizontal = artwork strip with Unsplash stock images (150×200 instead of the demo's 300×400) and figcaptions. Focus = 1 px `$ring` outline + 3 px `$ring-50` ring on a wrapper (pen.dev shadows have no spread) |
| Breadcrumb | `B5OZFZ` / `ijqPl` | `Breadcrumb/Link/{default,hover}`, `Breadcrumb/Page`, `Breadcrumb/Separator` (+`/slash`), `Breadcrumb/Ellipsis` (+`/size-4`), `Breadcrumb/{demo,hover,slash}` | none | List `gap-2.5` (desktop `sm:`), 14 `$muted-foreground`; link hover and page = `$foreground`. Separator = lucide `chevron-right` 14 (`[&>svg]:size-3.5`); custom separator example with `slash`. Ellipsis = `size-9` box with 16 px `ellipsis`; breadcrumb-demo shrinks it to `size-4`. Open scene: ellipsis DropdownMenu (Documentation / Themes / GitHub) from `DropdownMenu/Item` instances, `align start` |
| Alert | `El7Bb` / `x97CCC` | `Alert/default` (+`/title-only`, `/no-icon`), `Alert/destructive` (+`/title-only`, `/no-icon`) | `alpha/red-600-90`, `alpha/red-400-90`, `destructive-90` | `rounded-lg` (10, hardcoded) `border bg-card px-4 py-3`, 448 wide; with icon a 12 px column gap (`gap-x-3`), icon 16 nudged 2 px down (`translate-y-0.5`), without icon gap 0 (`grid-cols-[0_1fr]`). Title 14/500, `tracking-tight` (-0.35), single line; description 14 `$muted-foreground`, `[&_p]:leading-relaxed` (1.625), `gap-1`. Destructive: title and icon `$destructive`, description `$destructive-90` (`text-destructive/90`), still `bg-card`. Destructive example has the demo's bullet list (drawn with • text, no list-disc) |
| Sonner (toast) | `fSFmp` / `H4Vej` | `Sonner/Toast/{default,success,info,warning,error,loading,action}` + stacked Toaster preview | none | `sonner.tsx` only maps `--normal-bg` → `$popover`, `--normal-text` → `$popover-foreground`, `--normal-border` → `$border`, `--border-radius` → radius (10, hardcoded); no `richColors`, so every type is neutral and differs only by its lucide icon (circle-check, info, triangle-alert, octagon-x; loading = Spinner instance). Rest follows Sonner's default CSS (hardcoded): 356 wide, 16 px padding (13 left for the icon's -3 px margin), icon gap 10, 13 px title 500/1.5, description 400/1.4, shadow `0 4px 12px #0000001A`; action button 24 tall, 12/500, radius 4, inverted (`$popover-foreground` bg, `$popover` text). Stack preview is approximate (scaled 0.95 / 0.9 behind) |
| Progress | `ul2y5` / `j99jfn` | `Progress/{0,25,50,75,100}` | `alpha/black-20`, `alpha/neutral-200-20`, `primary-20` | Track `h-2` (8) `rounded-full` `$primary-20` (`bg-primary/20`), clipped; indicator `$primary` with width = value % of the 320 px track (code uses `translateX(-(100-value)%)` on a full-width bar; same visual). 0% hides the indicator. Width 320 stands in for the demo's `w-[60%]` |
| Slider | `D7JT9` / `udmmw` | `Slider/Thumb/{default,focus}`, `Slider/{default,range,focus,disabled,vertical}` | `slider-thumb-bg` (light `$white`, dark `$white`) | Track `h-1.5` (6) `rounded-full $muted`, Range `$primary`, 320 wide (demo `w-[60%]`). Thumb `size-4 rounded-full`, `border-primary`, `bg-white` → `$slider-thumb-bg`, `shadow-sm` (2 shadows); hover/focus = 4 px outer `$ring-50` (`ring-4 ring-ring/50`) on a wrapper. Thumb x = value × (width − 16) (Radix keeps thumbs inside the track). Range = 25–75 with two thumbs; disabled = opacity 0.5; vertical = `min-h-44` (176), range from the bottom |
| Item | `i2diPr` / `gCuBA` | `Item/Media/{default,icon,image}`, `Item/{default,outline,muted}/{default,sm}`, `Item/hover`, `Item/focus`, `Item/Group`, `Item/header-footer` | `alpha/neutral-700-50`, `accent-50` | Root `rounded-md` (radius/lg) transparent border; outline = `$border`, muted = `$muted-50`; size default `gap-4 p-4`, sm `gap-2.5 px-4 py-3`; 420 wide. Media: default (plain 20 px icon), icon (`size-8` `radius/md` border `$muted`, 16 px icon), image (`size-10`, Avatar stock photo). With a description, media is `self-start` + `translate-y-0.5`: drawn as a start-aligned row (2 px media nudge) with the actions vertically centered via computed top padding (pen.dev has no per-child align). Title 14/500 `leading-snug`, description 14 muted `leading-normal`. Actions = Button instances. Link hover = `$accent-50` (`[a]:hover:bg-accent/50`); focus = `$ring` border + 3 px `$ring-50`. Group = items with 1 px `$border` ItemSeparators (drawn) and Avatar instances; header/footer example uses Badge + Button instances |
| Sidebar (phase 1 of 2, not ticked) | `J0VJq` / `U7IigI` | `Sidebar/MenuButton/{default,hover,active,disabled,focus,icon,lg}`, `Sidebar/MenuBadge`, `Sidebar/GroupLabel`, `Sidebar/MenuSubButton/{default,active}`, `Sidebar/MenuSub`, `Sidebar/expanded` (w-64, 256), `Sidebar/collapsed` (icon, w-12, 48) | `alpha/black-70`, `alpha/neutral-50-70`, `sidebar-foreground-70` | MenuButton `h-8 p-2 gap-2 rounded-md` (radius/lg), 16 px lucide icons; hover/active = `$sidebar-accent` / `$sidebar-accent-foreground` (active also 500), disabled 0.5, focus = 2 px `$sidebar-ring`. MenuBadge `h-5 min-w-5 px-1` 12/500 (kept in the row instead of absolute right-1). GroupLabel `h-8 px-2` 12/500 `$sidebar-foreground-70`. MenuSub `mx-3.5 border-l $sidebar-border px-2.5 py-0.5 gap-1`, sub buttons `h-7 px-2`. Header/Footer `p-2 gap-2` with `lg` (h-12) team switcher (`$sidebar-primary` logo) and nav user (Avatar instance). Container `$sidebar` + right `$sidebar-border`, 720 tall. Collapsed: 32 px icon buttons, labels/badges/sub hidden, hover shows a `Tooltip/right` instance |
| Pagination | `kfQrU` / `hXRfO` | `Pagination/Link/{default,hover,active}`, `Pagination/Previous`, `Pagination/Next`, `Pagination/Ellipsis`, `Pagination/{demo,first,last}` | none | Built entirely from Button instances: Link = `Button/ghost/icon` (36) with the label shown and icon hidden, active = `Button/outline/icon` (`isActive`), hover = `$accent` override (Button hover still pending in Tier 0). Previous / Next = `Button/ghost/default` with `gap-1 px-2.5` and chevron; Next swaps the icon and label slots to put the chevron after the text. Ellipsis `size-9` with 16 px `ellipsis`. Buttons are `rounded-full` (library rule), so active page is a circle. `PaginationContent gap-1` |
| Navigation Menu | `ETKDn` / `gJKVK` | `NavigationMenu/Trigger/{default,hover,focus,open,disabled,link}`, `NavigationMenu/Link/{default,hover,active}`, `NavigationMenu/List`, `NavigationMenu/Content/{home,components}` | none (reuses `accent-50`, `muted-50`) | Trigger = `navigationMenuTriggerStyle` `h-9 px-4 py-2 rounded-md bg-background` 14/500, chevron 12 (`ml-1 top-px`); hover/focus `$accent`, open `$accent-50` with chevron rotated (drawn as chevron-up), focus 3 px `$ring-50` + 1 px `$ring` outline, disabled 0.5. Link `p-2 gap-1 rounded-sm` (radius/md), title 14/500 lh 1, description 14 muted `leading-snug` (`line-clamp-2` shown by truncating the Tabs text). Content (viewport={false}): `mt-1.5 rounded-md border bg-popover shadow p-2 pr-2.5`. Demo: Home open with gradient feature tile (`from-muted/50 to-muted` → `$muted-50` → `$muted`, `p-6`, row-span-3) + 3 links; Components 2-column panel (600). Indicator / viewport mode not drawn |
| Accordion (built by a parallel designer agent, not reviewed) | `Tnb5g` / `s9Bhj` | see frame | none expected (agent instructed not to create variables) | Built in parallel by a spawned designer agent from `accordion.tsx`; this session did not verify it. Review, then tick and replace this row with the details |
| Calendar | `Is0aP` / `MZqkl` | `Calendar/Day/{default,hover,focus,today,selected,range-start,range-middle,range-middle-first,range-middle-last,range-end,outside,disabled}`, `Calendar/Weekday`, `Calendar/WeekNumber`, `Calendar/Caption/label`, `Calendar/Dropdown/{default,focus}`, `Calendar/Nav/{previous,next}`, `Calendar`, `Calendar/dropdown`, `Calendar/week-numbers`, `Calendar/range` | none | `--cell-size` 32, root `p-3 $background`, month `gap-4`, weeks `mt-2` (gap 8); 248 wide (280 with week numbers, 488 for 2 months). Day = td cell holding a `Button/ghost/icon-sm` instance (label shown, 14 normal, `leading-none`), so it stays `rounded-full` (library rule) except where the code sets `rounded-md` (range start/end → `radius/lg`) or `rounded-none` (range middle; first/last cell of a week gets `rounded-l-md` / `rounded-r-md`). Today = td `rounded-md $accent`; selected = `$primary`; range start/end td `$accent` with left/right `radius/lg`, button `$primary`; outside = `$muted-foreground`; disabled = muted + 0.5. Hover `$accent` (same Button-hover gap as Pagination). Focus = 1 px `$ring` on the button + 3 px `$ring-50` outer on the cell. Weekday / WeekNumber 12.8 px (`0.8rem`) muted. Nav = absolute `inset-x-0` drawn in-row (prev + caption + next, 32 px spacer per month side in 2-month view); aria-disabled 0.5. Dropdown root `rounded-md border-input shadow-xs` (border via `spacing/px` padding), label `h-8 pl-2 pr-1 gap-1`, 14 px muted chevron; 34 px tall in a 32 px row, as in code. Demo month October 2026: `Calendar` = today 15, selected 20, outside days, days before today disabled; `Calendar/range` = Oct 26 – Nov 6 |
| Input OTP | `mYrUq` / `ywC0L` | `InputOTP/Slot/{empty,filled,active,active-filled,invalid,invalid-active}`, `InputOTP/Separator`, `InputOTP/Group`, `InputOTP` (+`/typing`, `/complete`, `/invalid`, `/disabled`, `/single-group`, `/three-groups`) | none (reuses `input-bg`, `ring-destructive`) | Two-layer slot like Input (root = 3 px `$ring-50` / `$ring-destructive` focus ring, Body = fill/border). Slot `h-9 w-9` (36), 14 `$foreground`, `$input-bg` (`dark:bg-input/30`), `shadow-xs`, `border-y border-r $input` (per-side strokeWidth); the group instance adds `first:border-l` + `first:rounded-l-md` / `last:rounded-r-md` (`radius/lg`). Active = `$ring` border + ring; empty active shows the fake caret (`h-4 w-px $foreground`, absolute centered, blink not drawn). Invalid = `$destructive` border, invalid-active ring `$ring-destructive`. Container `gap-2`, disabled = 0.5 on the whole InputOTP. Separator = 16 px lucide `minus`. Active slot's `z-10` can't be drawn: the next slot paints over the right edge of its ring (slots are transparent in light mode, so it stays visible) |
| Native Select | `Le4Ho` / `GRfRD` | `NativeSelect/Base` + `NativeSelect/{placeholder,filled,focus,disabled,invalid,invalid-focus,sm}` | none (reuses Input's `input`, `input-bg`, `ring`, `ring-50`, `destructive`, `ring-destructive`) | Two-layer like Input (root = 3 px focus ring, Body = `rounded-md` (`radius/lg`) `border-input` `$input-bg` `shadow-xs`). `h-9 px-3 py-2 pr-9` 14 `$foreground`; sm `h-8 py-1`. Width 133 = widest option ("Select status"; a native select sizes to its longest option, `w-fit` wrapper). Chevron 16 `$muted-foreground` at 50% opacity, absolute `right-3.5`. Placeholder = the `value=""` option, drawn in `$foreground` because `placeholder:` doesn't style a `<select>`. Focus `$ring` + `$ring-50`; invalid `$destructive` border, invalid-focus ring `$ring-destructive`; disabled 0.5 on the wrapper. Options / optgroups are OS-rendered (`bg-[Canvas]`) and not drawn |
| Chart | `u3wRI` / `NfOgT` | `Chart/Tooltip/{dot,line,dashed,hide-label,single-line,single-dashed,hide-indicator}`, `Chart/Legend/Item`, `Chart/Legend` (+`/top`, `/icons`), `Chart/bar-demo` | `chart-1`…`chart-5` rebound to `neutral/300, 500, 600, 700, 800` (both modes, shadcn `themes.ts` neutral); new `alpha/neutral-200-50`, `alpha/white-5`, `border-50` | Tooltip `min-w-[8rem]` drawn as fixed 128, `rounded-lg` 10 hardcoded, `border-border/50` → `$border-50`, `$background`, `px-2.5 py-1.5 gap-1.5`, `shadow-xl` approximated with 2 shadows `#0000001A` (no spread). Label 12/500, name 12 muted `leading-none`, value Geist Mono 12/500 (`font-mono tabular-nums`). Indicators `rounded-[2px]` = `radius/xs`: dot `size-2.5`, line `w-1` with the row height set explicitly (12, or 30 when the label is nested) instead of `items-stretch`, dashed = three 3 px dashes (no dashed strokes in pen.dev). Single series with a non-dot indicator nests the label (`items-end`). Legend `gap-4`, `pt-3` / `pb-3`, item `gap-1.5`, 8 px swatch, 12 px muted icons. bar-demo (chart-bar-demo-legend data): 480 wide, 200 tall plot (`min-h-[200px]`, not `aspect-video`), 5 `$border-50` grid lines (0–320 domain), bars `radius={4}` = `radius/sm`, band 10% gap + `barGap` 4, February cursor `$muted`, ticks muted with `tickMargin` 10, tooltip instance at the cursor. Swatch row for chart-1…5. In dark mode chart-4/5 (neutral 700/800) have low contrast on `$background`, same as shadcn's neutral theme |
| Carousel | `CPh5W` / `iIf8V` | `Carousel/{Previous,Next}` (+`/disabled`, `/vertical`), `Carousel/Slide` (+`/sm`, `/vertical`), `Carousel/horizontal` (+`/middle`), `Carousel/multiple`, `Carousel/vertical` | none | Previous / Next = `Button/outline/icon-sm` instances (`size-8 rounded-full`) with lucide `arrow-left` / `arrow-right`; vertical `rotate-90` drawn as `arrow-up` / `arrow-down`; disabled (`!canScrollPrev/Next`) = 0.5. Placed absolutely 48 px outside the root (`-left-12` / `-right-12`, vertical `-top-12` / `-bottom-12`), centered on the other axis. CarouselContent = clipped viewport with the track offset `-ml-4` (horizontal) or `-mt-1` (vertical demo); CarouselItem `pl-4` / `pt-1`. Slides = `p-1` + Card instance (Header/Footer hidden, CardContent `p-6` centered, number 36/600 or 30/600 `$card-foreground`). Demos: carousel-demo (`max-w-xs` 320, square content → 320 × 368, at slide 1 and slide 3), carousel-size (`max-w-sm` 384, `basis-1/3` → 112 × 160 slides), carousel-orientation (viewport 196 tall, items grow to content height 144, so slide 2 is cut off at the bottom, as in code) |
| Drawer | `dk96X` / `I6YLYi` | `Drawer/Handle`, `Drawer/Title`, `Drawer/Description`, `Drawer/Header` (+`/mobile`), `Drawer/Footer`, `Drawer/Demo Body`, `Drawer/Content/{bottom,top,right,left}`, `Drawer/demo` | none (reuses `overlay` via a `Dialog/Overlay` instance) | Content `$background`; bottom `rounded-t-lg` / top `rounded-b-lg` (10 hardcoded) with a 1 px `$border` top / bottom (per-side stroke); right / left `sm:max-w-sm` (384) with `border-l` / `border-r`, drawn 480 tall, footer pushed down (`mt-auto` → space_between). Handle (bottom only) `mt-4 h-2 w-[100px] rounded-full $muted`. Header `p-4`, desktop `md:gap-1.5 md:text-left`; `/mobile` = `gap-0.5 text-center` (bottom/top below md). Title 16/600 (no size class), Description 14 muted. Footer `p-4 gap-2` with full-width `Button/default` + `Button/outline` instances. drawer-demo: inner `max-w-sm`, goal row with `Button/outline/icon-sm` −/+ (`h-8 w-8 rounded-full`), 350 at 72/700 `tracking-tighter` (−3.6), `CALORIES/DAY` 11.2 muted, 13-bar chart 120 tall (`$foreground` at 0.9, domain 0–400) → 454 tall. Open scene: 960 × 640 page + `Dialog/Overlay` instance + demo drawer at the bottom. Drag / snap points not drawn |
| Accordion | `Tnb5g` / `s9Bhj` | `Accordion/Trigger/{closed,open,hover,focus,disabled}`, `Accordion/Content`, `Accordion/Item/{closed,open,last}`, `Accordion/demo` | none | Built by a parallel designer agent, reviewed 2026-10-15. Trigger `py-4 gap-4 rounded-md` (`radius/lg`) 14/500 `$foreground`, `items-start`, chevron 16 `$muted-foreground` with `translate-y-0.5` (2 px top padding); open = chevron `flipY` (`rotate-180`). Focus = 3 px `$ring-50` (`border-ring` has no border to color). Disabled 0.5. Item `border-b $border` via `spacing/px` padding + bottom stroke, `last:border-b-0`. Content `pb-4` 14 text, demo paragraphs `gap-4`. 384 wide. Hover (`hover:underline`) is drawn identical to closed: pen.dev drops the `underline` text property (even on plain text nodes), see Open follow-ups |
| Separator | `GOve5` / `Haxoq` | `Separator/horizontal`, `Separator/vertical`, `Separator/demo`; rebuilt `ButtonGroup/Separator` (`Bb3uz`) as a Separator instance | none | `bg-border` 1 px; horizontal `w-full` → `fill_container(240)`, vertical `h-full` → `fill_container(20)` (fallbacks for use outside a layout). Demo = separator-demo (`my-4`, `h-5` row with `space-x-4`). Swapped drawn lines for instances in `Field/Separator` (both lines), `Item/Group` (2 ItemSeparators), `ScrollArea/vertical` (10 tag separators, `my-2` wrappers kept), `InputGroup/textarea` (vertical, `!h-4` = 16). `ButtonGroup/Separator` was a reusable rectangle that couldn't become an instance: replaced by a new component (Separator/vertical + `bg-input` → `$input`, `self-stretch` → fill height in groups), its 2 instances repointed and the old rectangle (`RIiKM`) deleted. SidebarSeparator waits for Sidebar phase 2 |
| Skeleton | `h6afsk` / `al7TT` | `Skeleton` (line), `Skeleton/circle`, `Skeleton/block`, `Skeleton/pulse`, `Skeleton/demo`, `Skeleton/card` | none | `rounded-md` (`radius/lg`) `$accent`; size and radius come from `className` in code, so variants are instances: line `h-4 w-[250px]`, circle `size-12 rounded-full`, block `h-[125px] w-[250px] rounded-xl` (14 hardcoded). `animate-pulse` drawn as its low point (opacity 0.5). Demos: skeleton-demo (`space-x-4`, lines `space-y-2`, second line 200) and skeleton-card (`space-y-3`). Only Sidebar uses Skeleton (SidebarMenuSkeleton, phase 2), so no existing component needed updating |
| Select | `kFqVs` / `Pi3PM` | `Select/Trigger/Base` + `Select/Trigger/{placeholder,filled,focus,disabled,invalid,invalid-focus,sm}`, `Select/Item/{default,focus,selected,selected-focus,disabled}`, `Select/Label`, `Select/Separator`, `Select/ScrollUpButton`, `Select/ScrollDownButton`, `Select/Content` (+`/groups`, `/scrollable`); new `Field/vertical/select` (`kcvg8`) | none (reuses Input's `input`, `input-bg`, `ring`, `ring-50`, `destructive`, `ring-destructive`) | Trigger two-layer like Input: `rounded-md border-input $input-bg shadow-xs px-3 py-2 gap-2 h-9` (sm `h-8`), `justify-between`, chevron 16 muted at 50%; placeholder `$muted-foreground`, value `$foreground`; 180 wide (select-demo `w-[180px]`). Content: `rounded-md border $popover shadow-md` (same 2 shadows as DropdownMenu/Content), viewport `p-1` drawn as `[4,0]` padding + `px-1` groups so separators span edge to edge (`-mx-1`), width = trigger width. Item `py-1.5 pl-2 pr-8 gap-2 rounded-sm` (`radius/md`) 14, focus `$accent` / `$accent-foreground`, selected = 16 px check (`$muted-foreground`) in a 14 px box absolute at `right-2` (x fixed for the 180 content), disabled 0.5. Label `px-2 py-1.5` 12 muted. Separator `my-1` = Separator/horizontal instance. Scroll buttons `py-1` with 16 px chevrons. Open scenes: item-aligned (selected item over the trigger value) and popper (`translate-y-1`). Related update: `Field/vertical/select` (field-select demo, trigger `w-fit`) added to the Field frame and its dark preview; all frames right of Field shifted 368 px to keep spacing. No ui component renders Select, so nothing else to swap |

Still hardcoded in every component: the `shadow-xs` color `#0000000D`, disabled opacity `0.5`, font size, weight and line height (no typography primitives exist), and width/height (pen.dev can't bind them to variables).

## Open follow-ups

- [ ] Badge: link hovers (`[a&]:hover` primary/90, secondary/90, destructive/90, outline/ghost accent, link underline). Waiting on the Tier 0 hover variables.
- [ ] Button: rebind `Button/destructive/*` text and icon from hardcoded `#FFFFFF` to `$destructive-foreground`.
- [ ] Badge: consider renaming `button-destructive-bg` to `destructive-bg` now that Badge shares it.
- [x] Field, Button Group, Scroll Area, Input Group: replace drawn separator lines (`Field/Separator`, `ButtonGroup/Separator`, Scroll Area tag separators, Input Group textarea toolbar) with Separator instances once Separator is built. Done with Separator.
- [ ] Field: `FieldError` list mode (several errors as a bulleted `ul`) not drawn; only the single-message version exists.
- [x] Kbd: in-tooltip styling (`[[data-slot=tooltip-content]_&]`). Done in Tooltip with the approved `kbd-tooltip-bg`.
- [ ] Tabs: equal-width triggers (`flex-1` inside a `w-fit` list) are not the default. Instances are `fit_content`; set the trigger, Ring, Body and Content to `fill_container` in a fixed-width list to get them.
- [ ] Tabs: no hover state for the active trigger (same visuals as active) and no disabled-active combination.
- [ ] Tabs: `line` horizontal triggers overflow their 36 px Surface/List by 1 px (trigger is 34 px = 29 body + 3 gap + 2 indicator, placed at y 3). The indicator renders 1 px below the list; it isn't visibly clipped. Fix by trimming the gap to 2 px or letting the list height fit content.
- [ ] Sheet: reuse `$overlay` and `Dialog/Overlay` (same `bg-black/50`). Alert Dialog and Drawer already do.
- [ ] Radius: `rounded-lg` (10 px, Dialog) and `rounded-xl` (14 px, Card) have no primitive. Consider adding `radius/2.5` / `radius/3.5` or a shadcn-scale radius set.
- [ ] Avatar: new-york-v4 `Avatar` root has `overflow-hidden rounded-full`, which in the browser clips `AvatarBadge` (absolute bottom-right) to the circle. The design shows the badge unclipped, matching the radix base (no `overflow-hidden`) and the shared example. Check the running app and either drop `overflow-hidden` from the root in code or clip the badge in design.
- [ ] Sidebar phase 2 (after review): SidebarInput (Input instance), SidebarSeparator (Separator now built), SidebarGroupAction, SidebarMenuAction, SidebarMenuSkeleton (Skeleton now built), SidebarTrigger + Rail, SidebarInset, variants floating / inset, side right, offcanvas, mobile Sheet.
- [ ] Calendar: day hover is `$accent` in both modes; ghost `dark:hover:bg-accent/50` waits on the Button hover tokens. Not drawn: custom day content (`[&>span]:text-xs opacity-70`, e.g. prices), today + selected combination, `buttonVariant` other than ghost.
- [ ] Native Select, Select trigger: `dark:hover:bg-input/50` not drawn; add it together with the Input hover (`input-50`) in the Tier 0 hover task.
- [ ] Chart: only the bar chart is drawn as a container example (line, area, pie, radar, radial charts not drawn); tooltip `formatter` / custom-content demos not drawn.
- [ ] Underline: pen.dev drops the `underline` text property (tested on a plain text node), so `hover:underline` can't be drawn (Accordion trigger hover, Badge / Button link). Options: draw a 1 px line under the label inside the component, or wait for pen.dev support.
- [x] Menubar: reuses the `DropdownMenu/*` parts and `$menu-item-destructive-focus-bg`. Done.
- [x] Combobox: built after Input Group from `ui/combobox.tsx`.
- [ ] Context Menu: `ContextMenuTrigger` demo area uses `border-dashed`; pen.dev strokes can't be dashed, so it's drawn solid.

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

- [x] Select
  > Recreate `select.tsx` following the rules in `designs/components-checklist.md`. Include the trigger (placeholder, filled, focus, disabled, invalid), the open content with items, a selected item, a group label and a separator.

- [x] Field
  > Recreate `field.tsx` following the rules in `designs/components-checklist.md`. Build it from Label, Input, Textarea, Checkbox and Switch instances. Include description and error message, and vertical and horizontal orientation.

- [x] Badge
  > Recreate `badge.tsx` following the rules in `designs/components-checklist.md`. Include every variant.

- [x] Separator
  > Recreate `separator.tsx` following the rules in `designs/components-checklist.md`. Include horizontal and vertical.

- [x] Kbd
  > Recreate `kbd.tsx` following the rules in `designs/components-checklist.md`. Include a single key and a key group (e.g. Cmd + K).

- [x] Tooltip
  > Recreate `tooltip.tsx` following the rules in `designs/components-checklist.md`. Include the content with arrow on all four sides, and a version with a Kbd instance.

- [x] Dropdown Menu
  > Recreate `dropdown-menu.tsx` following the rules in `designs/components-checklist.md`. Include items (default, hover/focus, disabled, destructive), checkbox items, radio items, label, separator, shortcut, submenu and icons.

- [x] Dialog
  > Recreate `dialog.tsx` following the rules in `designs/components-checklist.md`. Include the overlay, header, title, description, footer with Button instances, and the close button.

- [x] Alert Dialog
  > Recreate `alert-dialog.tsx` following the rules in `designs/components-checklist.md`. Reuse Dialog's variables. Include header, footer with action and cancel Button instances, and every size it defines.

- [x] Tabs
  > Recreate `tabs.tsx` following the rules in `designs/components-checklist.md`. Include the list, triggers (active, inactive, hover, disabled) and content, in every variant and orientation.

- [x] Avatar
  > Recreate `avatar.tsx` following the rules in `designs/components-checklist.md`. Include image, fallback with initials, every size, and an avatar group if defined.

- [x] Skeleton
  > Recreate `skeleton.tsx` following the rules in `designs/components-checklist.md`. Include line, circle and card-shaped examples.

- [x] Spinner
  > Recreate `spinner.tsx` following the rules in `designs/components-checklist.md`. Include every size, and a Button instance in loading state.

---

## Tier 2: desktop app structure

- [x] Popover
  > Recreate `popover.tsx` following the rules in `designs/components-checklist.md`. Include the content with header, title and description if defined.

- [x] Command
  > Recreate `command.tsx` following the rules in `designs/components-checklist.md`. Include input, list, groups with headings, items (default, selected, disabled), shortcut, separator and empty state.

- [x] Combobox
  > Recreate `combobox.tsx` following the rules in `designs/components-checklist.md`. Use Popover and Command instances if it's built from them. Include closed, open, selected item and empty search.

- [x] Context Menu
  > Recreate `context-menu.tsx` following the rules in `designs/components-checklist.md`. Reuse Dropdown Menu's variables. Include items, checkbox and radio items, shortcut, separator and submenu.

- [x] Menubar
  > Recreate `menubar.tsx` following the rules in `designs/components-checklist.md`. Include the bar with triggers (default, open), and an open menu with items, shortcuts, separator and submenu.

- [x] Toggle
  > Recreate `toggle.tsx` following the rules in `designs/components-checklist.md`. Include every variant and size, off, on, hover, focus and disabled.

- [x] Toggle Group
  > Recreate `toggle-group.tsx` following the rules in `designs/components-checklist.md`. Use Toggle instances. Include single and multiple selection and every variant.

- [x] Button Group
  > Recreate `button-group.tsx` following the rules in `designs/components-checklist.md`. Use Button instances. Include horizontal, vertical, with separator and with text.

- [x] Input Group
  > Recreate `input-group.tsx` following the rules in `designs/components-checklist.md`. Use Input and Button instances. Include start and end addons (icon, text, button) and the textarea version.

- [x] Table
  > Recreate `table.tsx` following the rules in `designs/components-checklist.md`. Include header, rows (default, hover, selected), footer and caption. Show a row with a Checkbox and a Badge instance.

- [x] Scroll Area
  > Recreate `scroll-area.tsx` following the rules in `designs/components-checklist.md`. Include vertical and horizontal scrollbars.

- [x] Breadcrumb
  > Recreate `breadcrumb.tsx` following the rules in `designs/components-checklist.md`. Include link, current page, separator and ellipsis.

- [x] Alert
  > Recreate `alert.tsx` following the rules in `designs/components-checklist.md`. Include every variant, with and without icon, title and description.

- [x] Sonner (toast)
  > Recreate the toast from `sonner.tsx` following the rules in `designs/components-checklist.md`. Include default, success, error, warning, info, loading, and a toast with an action button.

- [x] Progress
  > Recreate `progress.tsx` following the rules in `designs/components-checklist.md`. Include 0%, 50% and 100%.

- [x] Slider
  > Recreate `slider.tsx` following the rules in `designs/components-checklist.md`. Include single and range, focus and disabled.

- [ ] Empty
  > Recreate `empty.tsx` following the rules in `designs/components-checklist.md`. Include media (icon), title, description and actions with Button instances.

- [x] Item
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

- [x] Accordion
  > Recreate `accordion.tsx` following the rules in `designs/components-checklist.md`. Include closed, open, hover and focus.

- [x] Calendar
  > Recreate `calendar.tsx` following the rules in `designs/components-checklist.md`. Include one month with today, selected day, range selection, outside days and disabled days.

- [x] Pagination
  > Recreate `pagination.tsx` following the rules in `designs/components-checklist.md`. Use Button instances if it's built from them. Include previous, next, active page and ellipsis.

- [x] Navigation Menu
  > Recreate `navigation-menu.tsx` following the rules in `designs/components-checklist.md`. Include triggers (default, open) and an open content panel with links.

- [x] Input OTP
  > Recreate `input-otp.tsx` following the rules in `designs/components-checklist.md`. Include empty, filled, active slot, separator and invalid.

- [x] Native Select
  > Recreate `native-select.tsx` following the rules in `designs/components-checklist.md`. Reuse Input's variables. Include placeholder, filled, focus, disabled and invalid.

- [x] Chart
  > Recreate the chart container, tooltip and legend from `chart.tsx` following the rules in `designs/components-checklist.md`. First, propose replacements for `chart-1` to `chart-5` (they still hold the docs site's blue ramp).

- [x] Carousel
  > Recreate `carousel.tsx` following the rules in `designs/components-checklist.md`. Include the previous and next buttons as Button instances, horizontal and vertical.

- [x] Drawer
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
