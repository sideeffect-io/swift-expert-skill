# Performance

Start with code review. Profile when review alone cannot prove the bottleneck.

## Code-First Review

Look for:

- redundant state writes
- broad dependencies that fan updates across many views
- unstable `ForEach` identity
- expensive work in `body`
- repeated sort, filter, or formatting work in row builders
- layout thrash from deep hierarchies or overused geometry measurement
- image decoding on the main thread
- root-level conditional view swapping
- expensive animations attached too high in the tree

## Hot-Path Rules

- Check whether a value actually changed before assigning state in a fast callback.
- Gate scroll- and geometry-driven UI by threshold.
- Move sorting, filtering, formatting, and image decoding out of `body`.
- Pass only the values a view needs instead of entire models.

```swift
.onScrollGeometryChange(for: Bool.self, of: { geometry in
    geometry.contentOffset.y < -32
}) { _, shouldShow in
    if shouldShow != showTitle {
        showTitle = shouldShow
    }
}
```

## Dependency Scope

- Narrow state to the leaf that owns it.
- Avoid making rows depend on large shared models if a smaller value or per-item model will do.
- Consider `equatable()` or smaller wrapper views only when measurement shows that it helps.

## Async Work

- Prefer `.task` for async view work so cancellation happens automatically when the view disappears.
- Do not start heavy asynchronous work from `body`.

```swift
.task(id: query) {
    await search(query)
}
```

## Debugging Unexpected Updates

- Use `Self._printChanges()` or `Self._logChanges()` in debug builds to identify why a view re-rendered.
- If a custom layout, `Shape.path`, `visualEffect`, or geometry callback can run away from the main actor, capture values instead of touching actor-isolated mutable state directly.

## Lazy Containers and Images

- Use `LazyVStack`, `LazyHStack`, or `LazyVGrid` for large or unbounded collections.
- Avoid decoding full-size images in list rows.
- Prefer placeholders and downsampled previews in scrolling contexts.

## When Review Is Not Enough

Guide the user to profile with Instruments:

- use a release build
- collect the SwiftUI instrument plus Time Profiler
- reproduce the exact interaction
- compare before and after for frame pacing, CPU, and update frequency

Ask for traces or screenshots when code review cannot identify the root cause with confidence.
