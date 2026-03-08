# Navigation, Presentation, and Input

Prefer typed, model-driven flows over boolean-heavy wiring.

## Navigation

- Prefer `NavigationStack` for push-style flows.
- Prefer `NavigationSplitView` for multi-column navigation, especially on iPad and macOS.
- Use `Hashable` route values and `navigationDestination(for:)` for typed navigation.
- Use `NavigationPath` only when you need heterogeneous path storage, restoration, or serialization.

```swift
enum Route: Hashable {
    case detail(Item.ID)
    case settings
}

struct RootView: View {
    var body: some View {
        NavigationStack {
            List(items) { item in
                NavigationLink(item.name, value: Route.detail(item.id))
            }
            .navigationDestination(for: Route.self) { route in
                switch route {
                case .detail(let id):
                    DetailView(id: id)
                case .settings:
                    SettingsView()
                }
            }
        }
    }
}
```

## Sheets and Modal Flows

- Prefer `.sheet(item:)` when the sheet represents selected model data.
- When multiple sheet types exist, use an `Identifiable` enum plus one sheet modifier.
- Let sheets own their own dismiss, save, and cancel actions unless the parent truly owns the workflow.

```swift
enum PresentedSheet: Identifiable {
    case add
    case edit(Item)

    var id: String {
        switch self {
        case .add:
            return "add"
        case .edit(let item):
            return "edit-\(item.id)"
        }
    }
}
```

## Alerts and Confirmation

- Attach alerts and confirmation dialogs close to the UI that triggers them.
- Prefer confirmation dialogs for choice-heavy destructive actions.
- Keep alert state small and specific.

## Forms and Text Input

- Prefer `Form` for grouped settings and structured data entry.
- Use `LabeledContent` to pair titles with controls in forms.
- Unless you need a larger freeform editing surface, prefer `TextField(axis: .vertical)` over `TextEditor` so you keep placeholder support and easier inline behavior.
- Use `@FocusState` for field-to-field navigation.

```swift
TextField("Notes", text: $notes, axis: .vertical)
    .lineLimit(3...8)
```

## Search

- Use `searchable(text:)` for native search UI.
- Add `searchScopes` only when multiple modes are genuinely useful.
- Debounce expensive searches with `.task(id:)` or model-level throttling.
- Use `ContentUnavailableView.search` or a custom empty state when results are empty.

## Scroll-Reveal and Secondary Surfaces

- If a screen reveals secondary content through scrolling, derive one normalized progress value from scroll state.
- Drive opacity, offset, blur, or affordance visibility from that one source of truth.
- Avoid layering a second gesture state machine on top unless scrolling cannot express the interaction.

## Platform Routing

- If the task includes macOS windows, settings, menu bar extras, inspectors, or split-view behavior, load `macos-scenes-and-windows.md`.
- If it includes `Table`, AppKit interop, file panels, or `NSViewRepresentable`, load `macos-views-and-interop.md`.
