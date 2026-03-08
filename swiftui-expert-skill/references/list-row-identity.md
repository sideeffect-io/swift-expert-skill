# List Row Identity

Read this file for unstable rows, `ForEach` bugs, or list diffing issues.

- `[Prefer]` Back rows with stable `Identifiable` models or explicit stable IDs.
- `[Prefer]` Keep one logical row shape per element; avoid changing child count across states inside one `ForEach`.
- `[Prefer]` Prefilter or presort outside the `ForEach` when the work is repeated often.
- `[Consider]` Prefer `List` when you need native selection, edit actions, and accessibility semantics.
- `[Only when]` Avoid `.indices`, unstable `id: \\.self`, and `AnyView` in repeated rows unless the data and identity model make that truly safe.
