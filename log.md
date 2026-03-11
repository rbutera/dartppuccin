## 2026-03-11 14:58 UTC — Mocha userstyle planning pass

**Task:** Create the initial planning artifacts for a Catppuccin userstyle port for dartai.com, scoped to a Mocha-first implementation, and preserve all reference links in the project README.
**Approach:** Reviewed the Catppuccin userstyle contribution docs, looked at several existing Catppuccin userstyle ports for structure, and inspected Dart's logged-in UI to identify the main surfaces that need coverage.
**Work done:**
- Created `README.md` with all user-provided reference links and implementation context
- Created `plan-mocha.md` with a Mocha-first implementation plan
- Created `implementation-strategy.md` describing the recommended override order across Tailwind semantic utilities, Vuetify variables, and AG Grid
- Reviewed Catppuccin reference ports for GitHub, YouTube, and Reddit
- Inspected Dart's tasks view and right-side issue detail panel in the logged-in browser
- Reviewed exported reference HTML/CSS and confirmed Dart ships Tailwind utilities plus semantic custom classes like `bg-std`, `text-lt`, and `border-md`
**Challenges:** The standalone task URL appeared to route back into the main split-view workspace, so the plan is based on the visible split panel plus route behavior rather than a clearly separate full-page task layout.
**Follow-up:** On implementation day, inspect for stable selectors or app-level CSS variables before writing component-specific overrides. Metadata should list Rai as maintainer in the eventual `userstyles.yml` entry.

## 2026-03-11 15:10 UTC — First Mocha userstyle implementation

**Task:** Build the first end-to-end Catppuccin Mocha userstyle for Dart and make it ready for manual testing.
**Approach:** Used the earlier plan plus the exported HTML/CSS reference files to theme the app in layers: semantic Tailwind utilities first, Vuetify variables second, and AG Grid plus detail-panel cleanup third.
**Work done:**
- Created `dartai-catppuccin-mocha.user.less`
- Added a proper userstyle metadata block and Catppuccin standard library import
- Mapped Dart semantic utilities like `bg-std`, `bg-lt`, `bg-md`, `text-lt`, `border-md`, and hover/focus helpers onto Catppuccin Mocha surfaces
- Patched Vuetify theme variables to Catppuccin-friendly values
- Added AG Grid variable overrides and focused row/menu/input styling for task-heavy views
- Added README test instructions for Stylus
**Challenges:** This is a broad first pass rather than a pixel-perfect final port. Dart mixes Tailwind utilities, Vuetify, AG Grid, and component-specific styling, so some dialogs or edge widgets may still need a second pass after real use.
**Follow-up:** Rai should install the `.user.less` file in Stylus and test the list view, task detail view, right-side issue panel, menus, and hover states. Next pass should tighten any selectors that miss or overreach.

## 2026-03-11 15:19 UTC — Second-pass polish for marked UI issues

**Task:** Patch the six marked problem areas from Rai's annotated screenshot.
**Approach:** Tightened container styling on the left-hand shell and main list pane, then reduced brightness and improved contrast for AG Grid micro-controls, selected-row embedded controls, and detail-panel divider/property surfaces.
**Work done:**
- Strengthened left sidebar container overrides to remove mismatched dark patches
- Added icon/control recoloring for AG Grid and button-adjacent micro-controls
- Improved selected-row pills and embedded action buttons so they read clearly against the accent-tinted row background
- Softened right-panel borders and property-strip separators
- Added broader pane/container background cleanup for list/detail split layouts
**Challenges:** Some fixes are still necessarily heuristic because Dart mixes semantic Tailwind classes with component-specific wrappers. The next test pass should reveal whether any selectors overreach or if one of the targeted controls still uses a more specific native rule.
**Follow-up:** Rai should refresh Stylus and recheck the six previously marked spots, especially the left rail seams, tiny circular controls, selected-row action chips, and right-panel subtask controls.

## 2026-03-11 15:35 UTC — Third-pass visual alignment toward vanilla Dart

**Task:** Fix the remaining issues from Rai's second screenshot set and bring the theme closer to Dart's native hierarchy instead of over-theming it.
**Approach:** Reduced contrast on sidebar section headers, removed boxed treatment from Tasks/Docs links in the left nav, separated hover from active states, flattened comments/description surfaces, and pulled AG Grid/status styling back toward neutral backgrounds with semantic colors preserved in the icons/text rather than heavy fills.
**Work done:**
- Tuned sidebar hover and active shades so they differ but stay in the same family
- Removed extra boxed/chip-like treatment from left-nav Tasks/Docs entries
- Lowered contrast on section headers like GENERAL and INCUBATION
- Neutralized dark fill in the task control area and AG Grid status column backgrounds
- Restored a more native look for status cells while keeping semantic status colors available
- Removed stray outline/filled treatment from description/comments area controls
- Flattened comment/activity/description surfaces closer to vanilla Dart
**Challenges:** This pass intentionally backs off some earlier, stronger Catppuccin styling choices. The trade-off is better fidelity to Dart's UI, but there may still be one or two places where semantic status color hooks need more precise selectors.
**Follow-up:** Rai should refresh the userstyle and compare against the vanilla screenshot, focusing on section-header contrast, left-nav item treatment, status column neutrality, and the description/comments surfaces.

## 2026-03-11 16:37 UTC — Release notes and strategy docs

**Task:** Add a small release-notes section to the README and write a durable strategy document so future maintainers can understand how this userstyle was built and debugged.
**Approach:** Captured the actual workflow used so far: layered theming, live DOM inspection, AG Grid-specific handling, selector misses, and the lessons learned from over-theming versus preserving Dart's vanilla hierarchy.
**Work done:**
- Added a compact release-notes section to `README.md`
- Created `docs/strategy.md` with the practical playbook for future iterations
- Documented the versioning convention, GitHub raw auto-update setup, selector/debugging workflow, and the main pitfalls encountered
**Challenges:** The most important part was writing down the mistakes honestly enough that Future Heimer or another maintainer can avoid repeating them.
**Follow-up:** Commit and push these docs changes in the next userstyle update cycle so the GitHub-hosted copy stays current.

## 2026-03-11 16:39 UTC — 0.0.6 live-selector tightening pass

**Task:** Proceed with version 0.0.6 and continue tightening the remaining sidebar and task-detail surface misses.
**Approach:** Used the live selectors already discovered to aim at the actual sidebar row wrappers and the real task-detail column/content wrappers, then reduced border weight and stripped accidental nested backgrounds from direct children.
**Work done:**
- Bumped the userstyle version to `0.0.6`
- Tightened sidebar row selectors around the real `Tasks`/space-name wrappers
- Forced sidebar large-chip/grid wrappers back to transparent so they stop reading as boxed panels
- Tightened task-detail wrapper styling and reduced the border weight on the detail column
- Added direct-child flattening for the title/properties/description/status-strip content groups
- Updated README release notes for `0.0.6`
**Challenges:** These are still heuristic live-class patches rather than a fully abstracted theme API. The remaining risk is that one more inner wrapper may still be carrying the visible background in the task view.
**Follow-up:** Refresh Stylus and check whether `0.0.6` finally changes the sidebar hierarchy and the task-detail sub-surfaces in the visible route.

## 2026-03-11 16:55 UTC — 0.0.7 exact-class patch pass

**Task:** Patch the still-missed Description button and sidebar toggle-section buttons using exact class strings supplied by Rai.
**Approach:** Stop guessing. Use the literal class patterns from Rai's copied HTML for two problem elements and align them to the adjacent surfaces instead of trying broader family selectors.
**Work done:**
- Bumped the userstyle version to `0.0.7`
- Targeted the exact `Description` button class and forced it onto the same background as the rightbar/editor surface
- Targeted the exact `dart-handle group/page-tab ... hover:bg-md` toggle-section button class and flattened it back to the sidebar surface
- Added explicit background alignment for `data-toolbar="rightbar-toolbar"` and inactive `.dart-editor`
- Updated README release notes for `0.0.7`
**Challenges:** Exact-class selectors are less elegant, but they are the fastest route when a live UI refuses to respond to broader semantic rules.
**Follow-up:** Refresh Stylus and verify whether the Description button and sidebar section-toggle buttons now visually merge with their surrounding surfaces.

## 2026-03-11 17:00 UTC — 0.0.8 status SVG color fix

**Task:** Stop overriding the semantic SVG color inside the live Status control button.
**Approach:** Use Rai's exact HTML snippet as the source of truth. The status icon already carries an inline `style="color: ..."`, so the correct fix is to remove the broad status-icon recolor from that button family and let the inline semantic color win.
**Work done:**
- Bumped the userstyle version to `0.0.8`
- Removed `Status` from the broad generic button/SVG recolor selector
- Added a narrow exception for `button[aria-label="Status"]` so its icon inherits its intended inline color instead of getting flattened
- Updated README release notes for `0.0.8`
**Challenges:** This confirmed the broader lesson from this project: generic icon rules are dangerous when the app is already encoding meaning through inline color.
**Follow-up:** Refresh Stylus and check whether the Status control now keeps its semantic green icon instead of getting recolored.

## 2026-03-11 17:02 UTC — 0.0.9 Lexical editor surface fix

**Task:** Stop inactive Lexical editors from keeping the darker editor background and only use that darker surface while the editor is active.
**Approach:** Use Rai's exact HTML snippet as the target. The editor is a `div[data-lexical-editor="true"][contenteditable="true"]`, so the safe rule is: transparent at rest, darker only on `:focus` / `:focus-visible`.
**Work done:**
- Bumped the userstyle version to `0.0.9`
- Added an exact-class and attribute-based selector for the Lexical task editor wrapper
- Set inactive editor background to transparent
- Set focused editor background to `@mantle`
- Updated README release notes for `0.0.9`
**Challenges:** Focus detection on contenteditable elements can be fiddly, but this is the right first move because the app is exposing a stable `data-lexical-editor` attribute.
**Follow-up:** Refresh Stylus and verify that the task editors now blend into the page at rest and only darken when actively focused.

## 2026-03-11 17:04 UTC — 0.0.10 right-edge row-control normalization

**Task:** Fix the dark patches on the right side of task rows caused by the open-rightbar control wrapper.
**Approach:** Use Rai's exact HTML snippet for the `dart-open-rightbar` region and flatten that wrapper back to transparent so the row surface shows through. Keep only a light border/icon treatment and a subtle hover wash on the control itself.
**Work done:**
- Bumped the userstyle version to `0.0.10`
- Targeted `.dart-open-rightbar` and its exact child control wrapper classes
- Removed the dark boxed background from the wrapper region
- Kept a faint border/icon style and a subtle hover effect for the actual control
- Updated README release notes for `0.0.10`
**Challenges:** This was another case where the visible dark patch lived in the wrapper utility classes, not in the icon itself.
**Follow-up:** Refresh Stylus and confirm that the right-edge patches on task rows now blend into the row background instead of appearing as separate dark blocks.

## 2026-03-11 17:09 UTC — 0.0.11 AG Grid wrapper + SVG override narrowing

**Task:** Continue fixing the dark right-edge row patches and stop the remaining unwanted SVG recoloring in the status column.
**Approach:** Expand the flattening rules from `.dart-open-rightbar` itself to the surrounding AG Grid cell wrappers, and narrow AG Grid cell SVG overrides so inline/app-provided icon colors are not stomped by broad button/icon rules.
**Work done:**
- Bumped the userstyle version to `0.0.11`
- Extended right-edge row-control flattening to `.ag-cell`, `.ag-cell-value`, `.ag-cell-wrapper`, and last-cell wrappers that contain `.dart-open-rightbar`
- Narrowed AG Grid cell SVG overrides so status-column icons can fall back to app-provided coloring rather than theme-imposed icon color
- Updated README release notes for `0.0.11`
**Challenges:** The AG Grid cell area mixes wrapper-level backgrounds with button/icon-level styling, so both layers had to be handled separately.
**Follow-up:** Refresh Stylus and check whether the right-edge dark patches are gone and whether the status-column SVGs stop being manually recolored.

## 2026-03-11 17:44 UTC — 0.0.12 live inline-variable patch

**Task:** Use live DOM evidence to fix the two remaining issues: right-edge dark patches and wrongly recolored Status SVGs.
**Approach:** Inspect the exact live DOM in the managed browser. The right-edge carrier turned out to be a wrapper with inline CSS variables `--background` and `--highlight`, and the Status button exposed its own CSS variables `--37b80b55` and `--73dfac23`. So the fix is to target those variables directly rather than fighting generic icon rules.
**Work done:**
- Bumped the userstyle version to `0.0.12`
- Forced the live `--background` / `--highlight` row-control wrapper back to transparent
- Set `button[aria-label="Status"]` SVG/icon color from its own `--73dfac23` variable
- Restored the Status button hover wash from its own `--37b80b55` variable
- Updated README release notes for `0.0.12`
**Challenges:** This confirmed that some of Dart's controls are driven less by ordinary class styling and more by inline CSS variables. Those need variable-aware overrides, not family-wide selectors.
**Follow-up:** Refresh Stylus and verify whether the right-edge patches are gone and the Status control now uses its intended variable-driven icon color again.

## 2026-03-11 19:36 UTC — 0.0.13 AG Grid status SVG selector fix

**Task:** Fix the remaining status-column icon flattening using Rai's devtools screenshots.
**Approach:** Use the Styles pane evidence directly. The screenshot showed the broad `.ag-cell button svg` rule from the userstyle still applying inside AG Grid cells and forcing status icons through `currentColor`. The correct fix is to exclude `button[aria-label="Status"]` from that AG Grid SVG override.
**Work done:**
- Bumped the userstyle version to `0.0.13`
- Narrowed the AG Grid SVG/icon override so it no longer applies to `button[aria-label="Status"]`
- Left the earlier status-specific variable-aware rules in place
- Updated README release notes for `0.0.13`
**Challenges:** This confirms the value of Rai's devtools screenshots: the wrong rule was visible in plain sight, and the bug was not solvable cleanly from screenshots of the UI alone.
**Follow-up:** Refresh Stylus and verify that the status column now preserves the native mixed icon colors instead of flattening them through the AG Grid cell SVG rule.

## 2026-03-11 19:40 UTC — 0.0.14 button SVG override removal

**Task:** Finish fixing the status-column icon flattening using Rai's JSON dump from the console.
**Approach:** Use the reported computed values as the source of truth. The dump showed two separate mistakes in the userstyle: (1) the broad generic `button svg` / `[role="button"] svg` override was still catching Status controls, and (2) the status-specific fix was wrongly forcing icons to `--73dfac23`, which turned out to be a white-ish variable rather than the semantic per-icon color. The correct fix is to stop overriding Status SVG color entirely and let the SVG's own inline `style="color: ..."` win.
**Work done:**
- Bumped the userstyle version to `0.0.14`
- Excluded `button[aria-label="Status"]` from the broad generic button/icon override
- Removed the mistaken status-specific color/fill/stroke override entirely
- Kept only the subtle Status hover background rule
- Updated README release notes for `0.0.14`
**Challenges:** This was a compounded bug: the first attempted fix reintroduced the problem in a different form because the CSS variable used for the icon was not the semantic status color.
**Follow-up:** Refresh Stylus and verify that the status-column icons now use their own native inline colors again.

## 2026-03-11 20:07 UTC — 0.0.15 right-edge title-bar flattening

**Task:** Fix the dark bars/panels to the right of task titles now that the status icons are corrected.
**Approach:** Reuse the live DOM evidence gathered earlier for the title row. The visible dark patches seem to live in the title-row carrier wrappers around `.dart-open-rightbar`, not just the tiny control itself. So this pass flattens the title-row wrapper chain directly: the `group/title` row, the padded flex container, and the first-column AG Grid title cell wrapper.
**Work done:**
- Bumped the userstyle version to `0.0.15`
- Flattened the title-row carrier wrappers around `.dart-open-rightbar`
- Removed background/background-image/box-shadow from the relevant title-row flex wrappers
- Kept the actual tiny right-edge control visible with subtle border/hover styling
- Updated README release notes for `0.0.15`
**Challenges:** The right-edge dark bars were not just the control box itself; they were being carried by one or more parent wrappers in the first title column.
**Follow-up:** Refresh Stylus and verify whether the bars beside task titles now blend into the task row instead of appearing as separate dark strips.

## 2026-03-11 20:12 UTC — 0.0.16 font stacks

**Task:** Add custom UI and monospace font stacks without interfering with SVG/icon rendering.
**Approach:** Set a UI font stack across normal HTML/text controls and app surfaces, set a separate mono stack for code/editor/code-like text, and explicitly avoid trying to style SVG/icon glyph systems as fonts.
**Work done:**
- Bumped the userstyle version to `0.0.16`
- Added UI font stack with `Geist Sans` first and `Inter` as fallback
- Added monospace stack with `JetBrainsMono Nerd Font` first and `JetBrains Mono` plus other nice monos as fallback
- Scoped the mono stack to code-like elements rather than the whole UI
- Left SVG/icon rendering out of the font change path
- Updated README release notes for `0.0.16`
**Challenges:** Font overrides can accidentally affect icon systems if applied too broadly, so this pass keeps the stacks on text-bearing HTML elements and code surfaces only.
**Follow-up:** Refresh Stylus and confirm that UI text uses Geist Sans/Inter and code-ish text uses JetBrainsMono Nerd Font/JetBrains Mono without disturbing icons.
