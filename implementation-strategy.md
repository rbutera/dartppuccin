# Dart Catppuccin implementation strategy

## Summary

Dart's frontend looks like a **hybrid stack**:
- Tailwind utilities and preflight
- a semantic utility layer built on top of Tailwind
- Vuetify theme variables
- AG Grid for task/table-heavy surfaces
- a few component-specific styles for split panes and detail views

That is good news. It means the userstyle should not start by styling individual widgets one by one. The clean route is to override the stack in layers.

## Verdict

**Tailwind helps here.** More importantly, Dart appears to ship **semantic utility classes** that are much better userstyle targets than raw layout utilities.

Examples found in the reference files:
- layout/utilities: `flex`, `grid`, `h-full`, `rounded`, `px-3`, `py-1`
- dark-mode utilities: `dark:*`
- semantic background/text/border utilities:
  - `bg-std`
  - `bg-lt`
  - `bg-md`
  - `text-lt`
  - `text-md`
  - `border-md`
  - `border-hvy`
  - `on:bg-md`

This means a Catppuccin port can probably recolor a large share of Dart by remapping **semantic classes first**, then patching the libraries underneath.

## Recommended override order

### Layer 1 — semantic Tailwind utilities
Start here.

Primary targets:
- `bg-std`
- `bg-lt`
- `bg-md`
- `bg-hvy` if present
- `text-lt`
- `text-md`
- `text-hvy` if present
- `border-lt`
- `border-md`
- `border-hvy`
- `on:bg-*` hover utilities
- shared focus helpers such as `focus-ring-*` if present

Why first:
- these classes appear across the app shell, sidebar, list rows, pills, buttons, and drawer surfaces
- they are more stable than targeting page-specific DOM chains
- they already encode the design system's intended surface hierarchy

### Layer 2 — Vuetify variables
Patch next.

Primary targets:
- `--v-theme-background`
- `--v-theme-surface`
- `--v-theme-surface-light`
- `--v-theme-surface-variant`
- `--v-theme-primary`
- `--v-theme-secondary`
- `--v-theme-error`
- `--v-theme-warning`
- `--v-theme-success`
- `--v-theme-on-*`
- opacity and border variables where useful

Why second:
- some controls and surfaces will ignore Tailwind utility remaps and inherit Vuetify colors instead
- variable remaps give broad coverage with very little CSS

### Layer 3 — AG Grid
Patch after the foundation is in place.

Primary targets:
- grid background
- headers and header separators
- rows and row hover
- selected row state
- cells and focus outlines
- menus and popovers
- inputs inside grid overlays
- pills, tags, and state badges rendered in rows

Why third:
- AG Grid has its own styling system and tends to need specific nudges
- once the global surface ladder is right, AG Grid patches become smaller and clearer

### Layer 4 — one-off component patches
Handle last.

Primary targets:
- split pane divider
- right-side issue panel shell
- rich text description blocks
- activity feed areas
- modals, tooltips, dropdowns
- odd route-specific wrappers in task detail pages

Why last:
- these are the pieces most likely to become brittle
- they should be written only after the shared tokens are settled

## Proposed Catppuccin mapping

### Surface ladder
- `bg-std` → `@base`
- `bg-lt` → `@mantle`
- `bg-md` → `@surface0`
- stronger selected/raised surfaces → `@surface1` or `@surface2`
- deepest chrome / nav edge / modal depth → `@crust`

### Text ladder
- `text-md` or primary text → `@text`
- `text-lt` or muted labels → `@subtext0`
- tertiary/disabled text → `@subtext1`, `@overlay1`, or `@overlay0`

### Borders and dividers
- `border-md` → `@surface1`
- `border-hvy` → `@surface2`

### Accents and semantic states
- primary accent → `@mauve` first, with `@blue` as fallback if mauve feels too decorative
- success → `@green`
- warning → `@yellow`
- danger → `@red`
- info / alternate emphasis → `@sapphire`

## Why this is better than selector-by-selector theming

A naive userstyle would style:
- sidebar item A
- button B
- row C
- drawer D

That works, but it ages badly.

The better route here is:
1. recolor the app's semantic utility classes
2. recolor framework variables
3. patch AG Grid
4. only then target stubborn components

That gives:
- better coverage
- fewer selectors
- less fragility when Dart ships new UI

## Practical implementation note

When building the real userstyle, inspect for these first:
- whether the app root carries `.dark`
- whether the main wrappers consistently use `bg-std`, `bg-lt`, `bg-md`
- whether buttons and list rows rely on `on:bg-*`
- whether AG Grid is using a stock theme class or custom tokens
- whether Vuetify variables are actually live on the task pages or only on some surfaces

## Recommendation for Future Heimer

Build the Mocha stylesheet in this order:
1. semantic Tailwind utilities
2. top-level text and selection colors
3. sidebar + app shell
4. task list and row states
5. right-side issue panel
6. AG Grid cleanup
7. popovers and edge cases

That should produce the highest-quality first pass with the least wasted effort.
