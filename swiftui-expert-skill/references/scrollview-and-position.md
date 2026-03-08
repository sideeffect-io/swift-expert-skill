# ScrollView and Position

Read this file for programmatic scrolling, scroll identity, or modern scroll-position APIs.

- `[Prefer]` Use `ScrollViewReader` for imperative jumps to known child IDs; it is available on iOS 14+, macOS 11+, tvOS 14+, watchOS 7+, and visionOS 1+.
- `[Prefer]` Keep scroll target IDs stable and colocated with the rendered items.
- `[Consider]` Use `scrollPosition(id:anchor:)` on iOS 17+ and macOS 14+ when you want a binding that tracks or drives a visible item ID.
- `[Consider]` When iOS 17+ or macOS 14+ ID-based scroll APIs depend on lazy-stack or grid children, mark the container with `scrollTargetLayout()`.
- `[Consider]` Use `ScrollPosition` on iOS 18+ and macOS 15+ for richer semantic positioning by ID, edge, or offset.
- `[Only when]` Do not store per-frame scroll offsets in general app state unless a feature truly depends on them.
