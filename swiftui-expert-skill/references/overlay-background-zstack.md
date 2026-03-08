# Overlay, Background, or ZStack

Read this file when the layout problem looks similar but the right primitive is unclear.

- `[Prefer]` Use `overlay` when decorating one primary view.
- `[Prefer]` Use `background` when the decoration belongs behind one primary view.
- `[Prefer]` Use `ZStack` when multiple peers jointly define layout.
- `[Consider]` Prefer modifier forms when the modified view should remain the layout anchor for alignment, hit testing, or sizing.
- `[Only when]` Switch to `ZStack` only if the decoration has become a real sibling with its own layout responsibility.
