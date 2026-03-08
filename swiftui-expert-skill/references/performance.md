# Performance

Start with code review. Profile when review alone cannot prove the bottleneck.

## Code-First Review [Prefer]

Look for:

- redundant state writes
- broad dependencies that fan updates across many views
- unstable `ForEach` identity
- expensive work in `body`
- repeated sort, filter, or formatting work in row builders
- expensive formatter construction in hot paths
- layout thrash from deep hierarchies or overused geometry measurement
- image decoding on the main thread
- root-level conditional view swapping
- expensive animations attached too high in the tree
- `AnyView` in repeated or performance-sensitive trees

## Hot-Path Rules [Prefer]

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

## Dependency Scope [Consider]

- Narrow state to the leaf that owns it.
- Avoid making rows depend on large shared models if a smaller value or per-item model will do.
- Consider `equatable()` or smaller wrapper views only when measurement shows that it helps.

## Async Work [Prefer]

- Prefer `.task` for async view work so cancellation happens automatically when the view disappears.
- Do not start heavy asynchronous work from `body`.

```swift
.task(id: query) {
    await search(query)
}
```

## Debugging Unexpected Updates [Consider]

- Use `Self._printChanges()` or `Self._logChanges()` in debug builds to identify why a view re-rendered.
- If a custom layout, `Shape.path`, `visualEffect`, or geometry callback can run away from the main actor, capture values instead of touching actor-isolated mutable state directly.

## Lazy Containers and Images [Prefer]

- Use `LazyVStack`, `LazyHStack`, or `LazyVGrid` for large or unbounded collections.
- Avoid decoding full-size images in list rows.
- Prefer placeholders and downsampled previews in scrolling contexts.

## Profiling Workflow [Prefer]

When code review is not enough, collect measurements before guessing:

1. Reproduce the issue in a release build on representative hardware.
2. Capture the SwiftUI instrument and Time Profiler for the exact interaction.
3. If the symptom is a stall or freeze, add the Hangs instrument.
4. Record device, OS version, build configuration, and reproduction steps.
5. Compare before and after CPU, frame pacing, update frequency, and memory peak.

Ask for traces or screenshots when a user reports jank, hangs, or CPU spikes without enough code context to prove the cause from inspection alone.

## When Review Is Not Enough [Only when]

Guide the user to profile with Instruments:

- use a release build
- collect the SwiftUI instrument plus Time Profiler
- add Hangs when the app freezes or appears unresponsive
- reproduce the exact interaction
- compare before and after for frame pacing, CPU, and update frequency

Ask for traces or screenshots when code review cannot identify the root cause with confidence.

## When Not to Apply [Only when]

- Do not treat every `GeometryReader` or custom layout as a bug; replace it only when a simpler modern API can express the same intent.
- Do not add `equatable()` or wrapper views blindly; use them only when profiling or update logging shows they help.
- Do not replace `List` with `ScrollView` for performance mythology alone; choose based on interaction model first.
