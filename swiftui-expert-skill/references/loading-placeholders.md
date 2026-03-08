# Loading Placeholders

Read this file for loading states, `redacted`, or preserving layout during incremental reloads.

- `[Prefer]` Keep the layout stable between loading and loaded states when that improves perceived continuity.
- `[Prefer]` Use `redacted(reason: .placeholder)` or lightweight skeletons when the final structure is already known.
- `[Prefer]` Use `ContentUnavailableView` for empty or unavailable content on iOS 17+, macOS 14+, tvOS 17+, watchOS 10+, and visionOS 1+, not for generic in-flight loading.
- `[Consider]` Overlay a progress indicator during incremental refresh instead of replacing the whole screen.
- `[Only when]` Use `ContentUnavailableView.search` for empty search results in a searchable hierarchy; Apple documents it as available on iOS 17+, macOS 14+, tvOS 17+, watchOS 10+, and visionOS 1+.
- `[Only when]` On older deployment targets, keep a lightweight custom empty-state view instead of building a partial clone of `ContentUnavailableView`.
