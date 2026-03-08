# Matched and Navigation Transitions

Read this file for hero transitions, `matchedTransitionSource`, or `navigationTransition`.

- `[Prefer]` Use `matchedTransitionSource(id:in:)` plus `navigationTransition(...)` for stable card-to-detail or row-to-detail transitions.
- `[Prefer]` Keep the source ID stable and scoped to one shared namespace.
- `[Prefer]` Use this only when source and destination represent the same semantic item.
- `[Consider]` Fall back to the default navigation transition when the hierarchy is too dynamic or the deployment target is earlier than iOS 18 or macOS 15.
- `[Only when]` Use older `matchedGeometryEffect` for non-navigation relationships; do not force it into push navigation when the newer API exists.
