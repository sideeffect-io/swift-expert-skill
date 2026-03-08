# Lists, Scrolling, and Media

Collection code tends to combine identity, layout, and performance issues. Keep it explicit.

## `ForEach` and Identity

- Prefer `Identifiable` models or stable explicit IDs.
- Do not use `.indices` for dynamic collections.
- Keep a constant number of rendered child views per element where possible.
- Avoid `AnyView` in rows.
- Avoid inline filtering or sorting inside `ForEach` when the work repeats frequently.

```swift
ForEach(items) { item in
    ItemRow(item: item)
}
```

## `List` vs `ScrollView`

- Use `List` for system row behavior, built-in reuse semantics, selection, edit actions, and accessibility.
- Use `ScrollView` with `LazyVStack`, `LazyHStack`, or `LazyVGrid` for custom feed layouts, horizontal strips, or mixed sections.
- Prefer `ScrollViewReader` with stable IDs for jump-to-item or scroll-to-top behavior.

```swift
ScrollView {
    LazyVStack(alignment: .leading, spacing: 12) {
        ForEach(items) { item in
            ItemRow(item: item)
                .id(item.id)
        }
    }
}
```

## Empty States and Search States

- Prefer `ContentUnavailableView` for empty or missing content states.
- For search-specific empties, prefer `ContentUnavailableView.search` when available.

## Tables

- Use `Table` for multi-column data. On compact size classes, only the first column remains visible, so make that first column informative.
- For richer macOS table behavior, sorting, selection, and AppKit-adjacent patterns, load `macos-views-and-interop.md`.

## Media and Remote Images

- Prefer `AsyncImage` or a project-approved image pipeline with explicit loading states.
- Keep full-resolution media out of list rows.
- If you manually decode image data, consider downsampling before rendering to reduce memory and main-thread cost.

```swift
AsyncImage(url: item.thumbnailURL) { phase in
    switch phase {
    case .success(let image):
        image
            .resizable()
            .scaledToFill()
    case .failure:
        Color.secondary.opacity(0.2)
    default:
        ProgressView()
    }
}
frame(width: 96, height: 96)
.clipShape(.rect(cornerRadius: 12))
```

## Scroll-Driven UI

- Gate state updates by threshold instead of assigning on every geometry change.
- Use `safeAreaInset` for sticky input bars and bottom actions.
- Keep IDs stable if a `ScrollViewReader` or programmatic scroll target depends on them.

## Collection Checklist

- Stable identity
- No hot-path transforms in `body`
- Correct container choice
- Cheap rows
- Explicit loading, empty, and error states
