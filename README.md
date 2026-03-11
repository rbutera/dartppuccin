# 24-dartppuccin

Catppuccin userstyle planning and implementation notes for `dartai.com`, with the first pass scoped to a **Mocha-only** port.

## Scope right now

This mini-project currently contains:
- a plan for the initial **Mocha palette** implementation
- source links and reference links Future Heimer should reuse when turning the plan into the actual userstyle
- notes from a quick visual pass over Dart's task list, full task view, and right-side issue detail panel

## Target app surfaces

Primary pages inspected for styling:
- Tasks board/list view: https://app.dartai.com/d/E6SmAat4KtlU-Tasks
- Task view reference: https://app.dartai.com/t/kF7HBjaGwSYF-FAIRY-DE-FRAZZLE-Investigate
- Side issue/detail panel: open an issue from the tasks view and style the right-hand drawer/panel as part of the same port

## Required Catppuccin userstyle docs

These are the pages Rai explicitly asked to preserve here for future implementation work:

- Creating userstyles: https://userstyles.catppuccin.com/contributing/creating-userstyles/
- `userstyles.yml` guide: https://userstyles.catppuccin.com/contributing/userstylesyml/
- Writing a userstyle: https://userstyles.catppuccin.com/contributing/tutorials/writing-a-userstyle/
- Syntax highlighting guide: https://userstyles.catppuccin.com/contributing/guides/syntax-highlighting/
- Inspect hard-to-grab elements: https://userstyles.catppuccin.com/contributing/tips-and-tricks/inspect-hard-to-grab-elements/
- Standard library: https://userstyles.catppuccin.com/contributing/standard-library/

## Palette reference

- Catppuccin palette reference: https://catppuccin.com/palette/

## Community reference repo

- Catppuccin userstyles repo: https://github.com/catppuccin/userstyles

## Community userstyles reviewed for structure

I examined these as reference points for how Catppuccin ports are typically organized:
- GitHub: https://github.com/catppuccin/userstyles/tree/main/styles/github
- YouTube: https://github.com/catppuccin/userstyles/tree/main/styles/youtube
- Reddit: https://github.com/catppuccin/userstyles/tree/main/styles/reddit

Why these three:
- **GitHub** is a good example of a large app with many semantic CSS variables and multiple page modes.
- **YouTube** shows how Catppuccin ports deal with app-level theme toggles and component-heavy pages.
- **Reddit** is a tidy example of remapping a site through exported CSS custom properties rather than painting every element one by one.

## Early observations about Dart's UI

From the logged-in browser pass, the current Dart UI appears to be a dark interface with these major regions:
- left navigation rail/sidebar
- list/table toolbar and filter row
- central task list with rows, chips, status dots, and assignee avatars
- right detail drawer/panel with title, properties, description, subtasks, and activity
- lots of thin dividers, muted text, and panel-on-panel layering

That means the first implementation should probably work in this order:
1. establish page-level background and text tokens
2. style surface hierarchy (`base` / `mantle` / `crust` / `surface0-2`)
3. map interactive states and accents
4. only then polish chips, badges, tables, drawers, and rich text blocks

## Maintainer note

Rai asked that the final Catppuccin metadata should list **him as maintainer, not me**. When the real port is prepared for the Catppuccin repo, the `userstyles.yml` entry should use Rai's GitHub handle in `current-maintainers`.

## Deliverables in this folder

- `README.md` — this file
- `plan-mocha.md` — implementation plan for the first Mocha-only pass
- `implementation-strategy.md` — theming strategy based on Tailwind semantic utilities, Vuetify variables, and AG Grid hooks
- `docs/strategy.md` — practical playbook for future theming/debugging iterations
- `dartai-catppuccin-mocha.user.less` — the first end-to-end Mocha userstyle to test in Stylus

## Tiny release notes

- `0.0.6` — tightened live-selector patches for sidebar row hierarchy and task-detail surface wrappers
- `0.0.5` — prepared the userstyle for GitHub raw auto-updates, pointed metadata at `rbutera/dartppuccin`
- `0.0.4` — first live-selector patch for the sidebar row wrappers and task-detail column wrappers
- `0.0.3` — flattened task-detail surfaces closer to vanilla Dart
- `0.0.2` — switched to `0.0.X` versioning for clearer live update tracking
- `0.0.1` — initial end-to-end Mocha userstyle pass

## Test instructions

1. Open Stylus.
2. Create a new userstyle from file, or paste in `dartai-catppuccin-mocha.user.less`.
3. Make sure it applies to `app.dartai.com`.
4. Check these three flows first:
   - task list / dashboard view
   - right-side issue detail panel
   - direct task route behavior
5. Watch for anything left too bright, too low-contrast, or semantically wrong in AG Grid rows, pills, menus, and dialogs.
