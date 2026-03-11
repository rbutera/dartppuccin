# Dart Catppuccin userstyle plan — Mocha first pass

## Goal

Create a Catppuccin userstyle for `app.dartai.com` that gives Dart a coherent **Mocha** look without fighting the app's information density.

This first pass is planning only. The implementation target is a working Mocha port that covers:
- the main tasks/list view
- the standalone task view
- the right-side issue detail panel opened from the list view

## Source references to use during implementation

Keep these open while building:
- Creating userstyles: https://userstyles.catppuccin.com/contributing/creating-userstyles/
- `userstyles.yml` guide: https://userstyles.catppuccin.com/contributing/userstylesyml/
- Writing a userstyle: https://userstyles.catppuccin.com/contributing/tutorials/writing-a-userstyle/
- Syntax highlighting guide: https://userstyles.catppuccin.com/contributing/guides/syntax-highlighting/
- Inspect hard-to-grab elements: https://userstyles.catppuccin.com/contributing/tips-and-tricks/inspect-hard-to-grab-elements/
- Standard library: https://userstyles.catppuccin.com/contributing/standard-library/
- Catppuccin palette reference: https://catppuccin.com/palette/
- Community repo: https://github.com/catppuccin/userstyles

Reference ports reviewed:
- GitHub: https://github.com/catppuccin/userstyles/tree/main/styles/github
- YouTube: https://github.com/catppuccin/userstyles/tree/main/styles/youtube
- Reddit: https://github.com/catppuccin/userstyles/tree/main/styles/reddit

## What the reference ports suggest

### 1) Follow the Catppuccin template exactly
The real port should start from the Catppuccin template layout, not a custom one-off stylesheet.

Expected shape:
- metadata block
- `@import "https://userstyles.catppuccin.com/lib/lib.less";`
- `@-moz-document ...` for the Dart domains
- a `#catppuccin(@flavor)` mixin
- `#lib.palette();`
- `#lib.defaults();`

For the first pass, even if only Mocha is being actively tuned, it still makes sense to keep Catppuccin's usual flavor variables in the metadata so the port fits upstream conventions.

### 2) Prefer semantic remapping over brute-force painting
The best reference ports do not scatter random hex values across hundreds of selectors. They:
- set semantic app variables where possible
- establish a surface ladder
- use direct element selectors only where the app gives them no clean variable hook

For Dart, that likely means:
- look for global CSS variables first
- if Dart exposes tokens, remap those to Catppuccin
- only fall back to component-level selectors for stubborn pieces

### 3) Use accent sparingly
Dart is a work surface, not a marketing site. The accent should signal action and focus, not flood the screen.

Default accent choice for the first pass:
- **Mauve** or **Blue** as the likely best fit

Decision to make during implementation:
- start with `mauve` if the aim is clearly Catppuccin-branded
- switch to `blue` if mauve makes status-heavy tables feel too decorative

## Visual mapping for Mocha

Use the Mocha palette as a hierarchy, not as a bucket of pretty colors.

### Base surface ladder
- app/page background → `@base`
- nested panels and drawers → `@mantle`
- high-emphasis containers, sticky bars, or deepest chrome → `@crust`
- hoverable cards / inputs / row hover → `@surface0`
- stronger boundaries / active containers → `@surface1`
- selected or raised elements needing extra separation → `@surface2`

### Text ladder
- primary text → `@text`
- secondary labels / metadata → `@subtext0`
- tertiary labels / placeholders → `@subtext1`
- disabled / divider-adjacent text → `@overlay1` or `@overlay0`

### Interaction colors
- links / focused actions / selected pills → `@accent`
- success / complete → `@green`
- warning / pending → `@yellow`
- danger / destructive / blocked → `@red`
- info / alternate emphasis → `@sapphire` or `@blue`

## Planned coverage by app region

### A. App shell
Style first:
- page background
- top toolbar/search bar
- left navigation sidebar
- section headers in the nav
- footer/settings/help area

Success condition:
- the shell reads as one coherent Mocha frame before any content styling starts

### B. Tasks list / dashboard view
Style second:
- filter bar, column headers, table/list grid lines
- task rows
- row hover state
- selected row state
- avatars
- pills/tags/status chips
- checkbox/radio/status dot controls

Success condition:
- dense rows stay readable, with clear contrast between idle, hover, and selected

### C. Right-side issue detail panel
Style third:
- panel background and border separation from main list
- title area
- metadata/properties grid
- rich-text description block
- subtasks section
- activity/comments area
- small buttons, chip badges, and inline controls

Success condition:
- the drawer feels like the same product, not a different theme pasted on the right edge

### D. Standalone task view
Style fourth:
- route-level layout background
- main content panel
- section headers
- editable fields and text inputs
- breadcrumbs or top-level route controls if present

Success condition:
- opening a task directly should preserve the same surface and text rules as the split view

## Implementation sequence

### Phase 1 — Scaffold the port
1. Fork `catppuccin/userstyles`.
2. Create a branch like `feat/dart` or whatever naming the maintainers prefer.
3. Copy the template into `styles/dart/` or `styles/dartai/` after checking repo naming conventions.
4. Fill in metadata for Dart.
5. Set domain targeting for `app.dartai.com`.

Open question for implementation:
- naming should likely match the product/site name used upstream. Decide between `dart` and `dartai` after checking nearby conventions.

### Phase 2 — Build a stable Mocha foundation
1. Import the standard library.
2. Wire `#catppuccin(@darkFlavor)` for Dart's dark UI.
3. Identify page-level containers and main panels.
4. Set the background ladder and text ladder.
5. Set border and divider colors.

This is the part that will make or break the rest. If the surface ladder is off, everything downstream turns muddy.

### Phase 3 — Style navigation and table mechanics
1. Sidebar links and active state
2. Search field and toolbar controls
3. Task table/list row states
4. Status markers, chips, and badges
5. Focus outlines and selected states

### Phase 4 — Style task detail surfaces
1. Right drawer shell
2. Description and body copy
3. Subtasks and activity blocks
4. Inline controls, buttons, and pills
5. Empty states and secondary widgets

### Phase 5 — Edge-case cleanup
1. Hover-only controls
2. Menus, popovers, and tooltips
3. Modals if encountered during inspection
4. Drag states / selected ranges if the app uses them
5. Any syntax-highlighted content, if Dart renders code blocks anywhere

If code blocks exist, check the syntax-highlighting guide and add `#lib.css-variables();` plus the appropriate Catppuccin syntax import.

## Inspection plan for implementation day

When Future Heimer starts writing the real stylesheet:

1. Open the tasks view.
2. Inspect app shell containers first.
3. Inspect one representative task row.
4. Open the right-side issue panel and inspect its root container.
5. Open the standalone task page and compare shared vs route-specific wrappers.
6. Use timed debugger tricks for hover menus or disappearing controls if needed:
   - reference: https://userstyles.catppuccin.com/contributing/tips-and-tricks/inspect-hard-to-grab-elements/

The thing to watch for is whether Dart uses stable semantic class names, CSS modules, generated hashes, or data attributes. If class names are unstable, prefer:
- data attributes
- ARIA labels
- DOM structure anchored to stable page landmarks
- CSS variables injected high in the tree

## Risks and design traps

### 1) Over-accenting the interface
If too many pills, icons, and active states use mauve, the UI will turn noisy fast.

Guardrail:
- reserve accent for primary action, selected state, links, and focus

### 2) Flattening surface contrast
Mocha is dark, but it still needs depth. If `base`, `mantle`, and `crust` are used interchangeably, the layout loses structure.

Guardrail:
- decide the surface ladder once, then apply it consistently

### 3) Fighting app-native status colors
Task apps often use color semantically for status and urgency. Repainting all statuses into one pastel family can destroy meaning.

Guardrail:
- keep semantic success/warning/error distinctions where they help scanning
- only harmonize saturation and contrast to fit Catppuccin

### 4) Chasing unstable selectors
Generated class names are a fine way to create future pain.

Guardrail:
- look for tokens, data attributes, ARIA hooks, and stable wrappers before writing long descendant chains

## Planned deliverables after the plan phase

When implementation starts, the first useful output should be:
- `styles/<port-name>/catppuccin.user.less` with Mocha-tuned rules
- notes on selector strategy and unstable areas
- a draft `userstyles.yml` entry
- maintainer set to **Rai** in the eventual upstream metadata, per request

## Definition of done for the Mocha pass

The first real implementation should count as done when all of this is true:
- tasks list view is readable and fully themed
- standalone task page is readable and fully themed
- right-side issue detail panel is themed and visually integrated
- hover, selected, focus, and disabled states are distinct
- tags, chips, dividers, inputs, and buttons match the Mocha hierarchy
- no glaring white/light panels remain in the inspected flows
- the file structure follows Catppuccin's userstyles conventions closely enough to upstream later

## Recommendation

Start with a **Mocha-only tuning pass**, but keep the Catppuccin template metadata intact so future expansion to Frappé, Macchiato, and Latte is mechanical rather than architectural.

That is the elegant route. Build the structure once. Tune the dark flavor well. Expand later.
