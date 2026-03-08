# Lists, Scrolling, and Media

Collection code tends to combine identity, layout, and performance issues. Keep it explicit.

## `ForEach` and Identity [Prefer]

- Prefer `Identifiable` models or stable explicit IDs.
- Do not use `.indices` for dynamic collections.
- Keep a constant number of rendered child views per element where possible.
- Avoid `AnyView` in rows.
- Avoid inline filtering or sorting inside `ForEach` when the work repeats frequently.
- For the shortest row-identity checklist, load `list-row-identity.md`.

```swift
ForEach(items) { item in
    ItemRow(item: item)
}
```

## `List` vs `ScrollView` [Prefer]

- Use `List` for system row behavior, built-in reuse semantics, selection, edit actions, and accessibility.
- Use `ScrollView` with `LazyVStack`, `LazyHStack`, or `LazyVGrid` for custom feed layouts, horizontal strips, or mixed sections.
- Prefer `ScrollViewReader` with stable IDs for jump-to-item or scroll-to-top behavior.
- For programmatic scroll and modern scroll-position bindings, load `scrollview-and-position.md`.

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

## Empty States and Search States [Prefer]

- For generic loading, empty, and placeholder surfaces, load `loading-placeholders.md`.
- For search-specific empties, prefer `ContentUnavailableView.search` when available and load `search-and-scopes.md` when search UI is part of the flow.

## Tables [Prefer]

- Use `Table` for multi-column data. On compact size classes, only the first column remains visible, so make that first column informative.
- For sortable `Table`, file dialogs, and macOS-heavy table nuance, load `macos-table-and-file-panels.md`.
- For AppKit interop around tables or hosted views, load `macos-views-and-interop.md`.

## Media and Remote Images [Prefer]

- Prefer `AsyncImage` or a project-approved image pipeline with explicit loading states.
- Keep full-resolution media out of list rows.
- If you manually decode image data, consider downsampling before rendering to reduce memory and main-thread cost.
- Prefer `ImageRenderer` over legacy UIKit or AppKit renderers when you need to export a SwiftUI view as an image.
- For `AsyncImage` and downsampling nuance, load `asyncimage-and-downsampling.md`.

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
.frame(width: 96, height: 96)
.clipShape(.rect(cornerRadius: 12))
```

## Scroll Coordination [Consider]

- Gate state updates by threshold instead of assigning on every geometry change.
- Keep IDs stable if a `ScrollViewReader` or programmatic scroll target depends on them.
- For sticky bars, placeholder states, or scroll-driven chrome, load `input-bars-and-safe-area-insets.md`, `loading-placeholders.md`, and `navigation-presentation-and-input.md`.

## Collection Checklist

- Stable identity
- No hot-path transforms in `body`
- Correct container choice
- Cheap rows
- Explicit loading, empty, and error states
