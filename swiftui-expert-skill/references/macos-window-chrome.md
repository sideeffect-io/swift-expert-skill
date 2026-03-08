# macOS Window Chrome

Read this file for scene-level window sizing, toolbar styling, and singleton versus multiwindow choices.

- `[Prefer]` Configure default macOS window behavior at the scene level with modifiers such as sizing, resizability, and toolbar style.
- `[Prefer]` Use `WindowGroup` for repeatable windows and `Window` or `UtilityWindow` only when the UX is truly singleton or utility-like.
- `[Consider]` Keep window chrome decisions aligned with the content model: document windows, utility palettes, and sidebars should not all share the same defaults.
- `[Only when]` Load the larger macOS references for exact API and availability details before writing version-specific code.
