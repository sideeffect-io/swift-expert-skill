# macOS Views and Interop

Read this file only when the task includes macOS-only view APIs, tables, file panels, or AppKit interop.

For dense optional detail on tables and file dialogs, also load `macos-table-and-file-panels.md`.

## Split Views

- Use `NavigationSplitView` for sidebar-driven navigation.
- Use `HSplitView` and `VSplitView` for peer-pane layouts such as editor plus preview or editor plus console.

```swift
HSplitView {
    SidebarView()
        .frame(minWidth: 220)
    EditorView()
        .frame(minWidth: 420)
}
```

## Tables

- `Table` is available cross-platform, but macOS gives the fullest multi-column experience.
- Support sorting with `sortOrder` and explicitly re-sort your backing collection.
- Support selection with a single ID or `Set<ID>`.
- Make the first column informative for compact environments where later columns collapse.
- For the dense availability and file-panel summary, load `macos-table-and-file-panels.md`.

```swift
@State private var sortOrder = [KeyPathComparator(\Person.givenName)]

Table(people, sortOrder: $sortOrder) {
    TableColumn("Given Name", value: \.givenName)
    TableColumn("Family Name", value: \.familyName)
}
.onChange(of: sortOrder) { _, newOrder in
    people.sort(using: newOrder)
}
```

## File Panels and Clipboard

- Prefer `fileImporter`, `fileExporter`, and `fileMover` before dropping to AppKit panels directly.
- Use `PasteButton` and, where available, `CopyButton` for system clipboard actions.
- Customize file dialogs with the SwiftUI modifiers that the deployment target supports before wrapping AppKit.
- For security-scoped import details and `Table(sortOrder:)` availability, load `macos-table-and-file-panels.md`.

## Drag and Drop

- Prefer `draggable` / `dropDestination` for modern transfer flows.
- Use legacy `onDrag` / `onDrop` only when compatibility or an existing codebase requires it.
- Favor `Transferable` models over raw pasteboard plumbing when possible.

## AppKit Interop

- Use `NSViewRepresentable` for AppKit views and `NSViewControllerRepresentable` for AppKit view controllers.
- Implement `makeNSView`, `updateNSView`, and cleanup behavior deliberately.
- Use a coordinator when delegate or target-action callbacks must feed SwiftUI state.
- Let SwiftUI own layout; do not fight it by manually mutating frame and bounds on the hosted AppKit view.

```swift
struct LegacyView: NSViewRepresentable {
    func makeNSView(context: Context) -> NSScrollView {
        NSScrollView()
    }

    func updateNSView(_ view: NSScrollView, context: Context) {
        // Sync SwiftUI state into AppKit here.
    }
}
```

## Hosting Boundaries

- Use `NSHostingView` or `NSHostingController` when embedding SwiftUI in existing AppKit code.
- Keep the ownership boundary explicit: AppKit owns the host, SwiftUI owns the subtree inside it.
