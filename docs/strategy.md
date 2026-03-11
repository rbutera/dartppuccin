# Strategy for building and iterating the Dart Catppuccin userstyle

This document is the practical playbook for future work on `dartai-catppuccin-mocha.user.less`.

It is not just a summary of what the theme is supposed to look like. It records the actual methods, working assumptions, mistakes, and recovery techniques used while building the first live version.

## Goal

Theme Dart with Catppuccin Mocha **without losing Dart's own UI hierarchy**.

That matters more than aggressively recoloring every surface. The best result is not “maximum Catppuccin paint.” The best result is “this still looks like Dart, but calmer, warmer, and more coherent.”

## Core design rule

**Prefer fidelity to Dart's native information hierarchy over thematic purity.**

When those conflict:
- keep surface depth similar to vanilla Dart
- keep interaction states readable
- keep semantic status meaning intact
- use Catppuccin as a remap, not a bulldozer

## What the stack appears to be

From the exported files and live inspection, Dart is a hybrid of:
- Tailwind utilities
- semantic utility classes built on top of Tailwind
- AG Grid for task/list surfaces
- additional app-specific wrappers and component styling
- some Vuetify variables in the shipped page

This means the theme should be written in **layers**, not as a pile of unrelated selectors.

## The working order of operations

### 1. Start with semantic utility classes
These were the most valuable early hooks.

Examples found in the live/exported files:
- `bg-std`
- `bg-lt`
- `bg-md`
- `bg-hvy`
- `text-lt`
- `text-md`
- `border-md`
- `border-hvy`
- `hover:bg-lt`
- `on:bg-*`

Why they matter:
- they map to surface hierarchy
- they repeat across the app
- they are much safer than styling random layout containers blindly

### 2. Patch AG Grid separately
AG Grid is its own system. It should not be treated like ordinary page markup.

Useful AG Grid hooks included:
- `--ag-background-color`
- `--ag-header-background-color`
- `--ag-row-hover-color`
- `--ag-selected-row-background-color`
- `--ag-checkbox-*`
- `.ag-row`
- `.ag-cell`
- `.ag-row-selected`
- `.ag-header`
- `.ag-menu`
- `.ag-checkbox-input-wrapper`

Key lesson:
- set AG Grid variables first
- then patch the few pieces that still escape, such as status icons, menus, and selected-row controls

### 3. Only then patch route-specific wrappers
This is where the early passes went wrong.

Broad selectors like:
- `[class*="detail"]`
- `[class*="drawer"]`
- `[class*="rightbar"]`

were useful for a first pass, but not reliable enough for finishing work.

They often *looked* plausible, but they missed the actual live wrappers on the task page.

The successful route-specific patching came only after inspecting the exact live class strings.

## Critical lesson: exported CSS was useful, but not enough
The `reference/` folder helped identify:
- Tailwind presence
- semantic utility names
- AG Grid usage
- general structure

But it was not enough to finish the theme, because several stubborn problems required **live DOM inspection**.

Examples of misses caused by relying too much on exported CSS:
- sidebar headers not matching the real live structure
- task-detail title / properties / description surfaces not changing
- status-strip and detail-column wrappers not being hit by generic selectors

## What actually worked for debugging

### A. Use live DOM inspection, not just screenshots
When a patch “should” have worked but nothing changed, the issue was usually one of:
- wrong selector
- too-broad selector hitting the wrong element
- missing a more specific live wrapper

The fix was to inspect the exact element in the live browser and walk up its parent chain.

### B. Inspect parent chains for target text
A reliable method was:
1. find a node by visible text, like `Tasks`, `Description`, `Properties`, or `Assignee`
2. walk up 6-12 parent levels
3. capture:
   - tag
   - class name
   - computed background color
   - computed text color
   - computed border color

This exposed the real wrappers and the actual surface carrying the unwanted color.

### C. Compare “what user sees” vs “what rule hits”
Important distinction:
- the text node is rarely the surface you need to recolor
- usually the visible surface lives 2-5 parents higher

That was especially true for:
- the task detail column
- the status/properties strip
- left sidebar row wrappers

## Live-inspection technique that worked

Use browser evaluation to:
- locate visible text
- return parent chains
- return computed styles

What to inspect next time:
- one sidebar section header
- one sidebar item row
- task title text
- `Description`
- `Properties`
- `Assignee`
- one status icon in the list

## Specific selector discoveries that mattered

### Sidebar item rows
A real live wrapper for sidebar items looked like:
- `flex w-full select-none items-center gap-1 truncate rounded text-md ... border border-transparent hover:bg-lt`

That told us:
- Tasks/Docs rows are normal row wrappers, not special chips
- if they looked boxed, the theme was over-styling them
- hover treatment should remain subtle and close to vanilla

### Task detail page wrapper
A key live wrapper for the task-detail column turned out to be:
- `flex size-full justify-center bg-std border-l border-md`

That was the missing piece when earlier generic detail selectors did nothing.

### Properties / Description area
Useful live classes included:
- `ml-3.5 mr-4 flex items-center justify-between`
- `mx-auto flex min-h-full w-full max-w-3xl flex-col gap-6`
- `flex w-full flex-col gap-6`
- `dart-large-chips container mt-1 w-full px-[13px] text-sm text-md`
- `grid grid-flow-row auto-rows-max gap-4`

These were much more useful than broad “detail” guesses.

## Visual problems encountered and their causes

### 1. Sidebar headers too strong
Cause:
- over-styled muted labels as if they were active content

Fix:
- keep section headers in `overlay1` / muted text
- keep row labels in normal text color

### 2. Tasks / Docs looked boxed
Cause:
- generic chip/button styling leaked into plain sidebar rows

Fix:
- explicitly reset those sidebar row wrappers to transparent background and transparent borders unless hovered/active

### 3. Status column lost semantic meaning
Cause:
- a broad icon recolor flattened all AG Grid controls to the same grey

Fix:
- reintroduce semantic icon colors specifically for status controls
- keep background neutral, but color the icon itself by state

### 4. Comments / description too dark
Cause:
- broad dark panel styling got applied to content sections that should have been flatter

Fix:
- reduce those sections to transparent or base-level surfaces
- remove accidental box-shadow and extra panel framing

### 5. Top segmented control lost hover state
Cause:
- panel-button flattening removed hover wash along with heavy outlines

Fix:
- restore hover and active states separately for that control family

### 6. Pane dividers too heavy
Cause:
- split-pane separators were given too much contrast and thickness

Fix:
- make them a thin, quiet separator close to vanilla Dart

## Practical theming heuristics that helped

### Surface ladder
Use these roughly:
- page base → `@base`
- quiet secondary surfaces → `@mantle`
- hover / subtle chips → `@surface0`
- stronger active surfaces → `@surface1`
- dividers / strong control edges → `@surface2`

### But do not force a panel where vanilla Dart has none
This was the most important restraint.

If Dart shows a section as part of the page background, keep it there.

### Status meaning should be in the icon or text first
Not in large background fills.

Recommended pattern:
- neutral background
- semantic icon color
- optional light tint only if needed for selected state or emphasis

### Hover and active must be distinct
Especially in the left sidebar.

If hover and active look the same, the UI loses orientation.

## Suggested workflow for future edits

1. Make a small visual change.
2. Bump `@version` from `0.0.X` to `0.0.(X+1)`.
3. Commit and push.
4. Refresh Stylus.
5. Compare against live vanilla Dart or a reference screenshot.
6. If the change does not appear, assume **selector miss first**, not palette problem.
7. Inspect live DOM before trying another guess.

## Versioning convention
Use:
- `0.0.1`
- `0.0.2`
- `0.0.3`
- etc.

This makes it easy to tell whether Stylus has picked up the latest revision.

## Auto-update setup
The userstyle is prepared for GitHub raw auto-update.

Expected URLs:
- homepage: `https://github.com/rbutera/dartppuccin`
- update URL: `https://raw.githubusercontent.com/rbutera/dartppuccin/main/dartai-catppuccin-mocha.user.less`

Recommended update flow:
1. edit file
2. bump `@version`
3. commit
4. push
5. let Stylus refresh from raw GitHub URL

## What someone else should do next time

If you are taking over this theme:

1. Start by reading the current userstyle file.
2. Compare the current Dart UI to the existing theme.
3. Do not trust exported CSS alone.
4. Inspect the live DOM for any surface that refuses to change.
5. Patch the exact wrapper carrying the visible background.
6. Keep Dart's own visual hierarchy intact wherever possible.

## Short rulebook

- do not over-theme
- keep vanilla hierarchy
- preserve semantic status meaning
- use live selectors when broad ones fail
- AG Grid is special; treat it separately
- sidebar rows are not pills
- if a change doesn't show up, inspect the live DOM before touching colors again
