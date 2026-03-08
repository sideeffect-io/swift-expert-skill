# Title Menus and Toolbars

Read this file for dense action surfaces in navigation bars, title menus, or macOS-style command density.

- `[Prefer]` Put primary frequent actions directly in the toolbar; move secondary actions into `Menu`.
- `[Prefer]` Use `toolbarTitleMenu` when the title itself is a contextual switcher or action surface.
- `[Prefer]` Keep toolbar actions semantic and stable; avoid rebuilding placements from unrelated booleans.
- `[Consider]` Promote one action out of a menu only when telemetry or product requirements show it is truly primary.
- `[Only when]` Load macOS references for menu bar extras, commands, or window-centric toolbar behavior.
