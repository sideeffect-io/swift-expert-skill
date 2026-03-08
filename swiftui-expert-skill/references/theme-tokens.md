# Theme Tokens

Read this file for semantic colors, shared spacing, and architecture-agnostic theming.

- `[Prefer]` Use semantic tokens for color, spacing, type, and radius rather than repeating literals across views.
- `[Prefer]` Put visual tokens in asset catalogs, environment values, or explicit value types passed through the tree.
- `[Prefer]` Use `tint`, `foregroundStyle`, materials, and asset colors before bespoke color math.
- `[Consider]` Extract `ButtonStyle`, `LabelStyle`, `ToggleStyle`, or small modifiers when the shared styling encodes interaction or semantics, not just paint.
- `[Only when]` Avoid a mutable singleton theme manager unless the existing codebase is already built around it.
