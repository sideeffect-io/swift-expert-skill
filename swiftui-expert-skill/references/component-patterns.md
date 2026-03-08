# Component Patterns

Read this file when the task is about concrete SwiftUI building blocks such as tabs, grids, toolbars, menus, placeholders, previews, or haptics.

## Tabs [Prefer]

- Use `TabView(selection:)` with a typed enum instead of integer or string tags.
- Prefer the modern `Tab("Title", systemImage: ..., value: ...)` API over `tabItem()`.
- If the product expects each tab to preserve its own navigation history, keep separate navigation state per tab instead of one shared path for the whole app.
- Keep tab identity stable. Do not rebuild the whole tab container from unrelated booleans.

```swift
enum AppTab: Hashable {
    case home
    case favorites
    case settings
}

struct RootTabs: View {
    @State private var selectedTab: AppTab = .home

    var body: some View {
        TabView(selection: $selectedTab) {
            Tab("Home", systemImage: "house", value: .home) {
                HomeView()
            }
            Tab("Favorites", systemImage: "star", value: .favorites) {
                FavoritesView()
            }
            Tab("Settings", systemImage: "gearshape", value: .settings) {
                SettingsView()
            }
        }
    }
}
```

## Grids and Adaptive Layouts [Prefer]

- Use `Grid` for small static two-dimensional layouts.
- Use `LazyVGrid` or `LazyHGrid` for large or scrollable grid content.
- Prefer adaptive columns when the layout should respond to available width.
- Keep grid cells cheap and stable, just like list rows.
- Prefer layout inputs such as size classes, container size, or `containerRelativeFrame` over hard-coded screen math.
- For choosing native form controls inside grouped data entry, load `form-controls.md`.

```swift
private let columns = [
    GridItem(.adaptive(minimum: 120, maximum: 180), spacing: 12)
]

ScrollView {
    LazyVGrid(columns: columns, spacing: 12) {
        ForEach(items) { item in
            ItemCard(item: item)
        }
    }
}
```

## Toolbars, Menus, and Secondary Surfaces [Prefer]

- Use `toolbar` and typed `ToolbarItem` placements for top-bar actions.
- Use `Menu` for secondary actions that do not deserve constant on-screen affordance.
- Use `toolbarTitleMenu` when the title itself should expose contextual actions or destinations.
- Use `safeAreaInset` for sticky bottom actions, input bars, and floating controls tied to a scroll container.
- Use `overlay` or `background` when decorating a primary view; use `ZStack` when multiple peers define layout together.
- Attach `confirmationDialog` close to the triggering control so the state and source of action stay obvious.
- For title menus and denser toolbar triage, load `title-menus-and-toolbars.md`.
- For sticky input bars, load `input-bars-and-safe-area-insets.md`.
- For layout ownership between decoration and peer content, load `overlay-background-zstack.md`.

```swift
NavigationStack {
    DocumentList()
        .toolbar {
            ToolbarItem(placement: .topBarTrailing) {
                Menu("Options", systemImage: "ellipsis.circle") {
                    Button("Sort by Name") { sortMode = .name }
                    Button("Sort by Date") { sortMode = .date }
                }
            }
        }
        .safeAreaInset(edge: .bottom) {
            ComposeBar(text: $draft)
                .padding()
                .background(.thinMaterial)
        }
}
```

## Loading, Empty, and Placeholder States [Prefer]

- Prefer `ContentUnavailableView` for empty or missing data states.
- Use `redacted(reason: .placeholder)` or lightweight skeletons when you want to preserve layout during loading.
- Prefer keeping the existing content visible with an overlayed progress indicator when reloading incremental data.
- Avoid rebuilding the whole screen tree on every loading transition if the content identity has not actually changed.
- For the compact decision summary, load `loading-placeholders.md`.

```swift
List(items) { item in
    ItemRow(item: item)
}
.overlay {
    if isReloading {
        ProgressView()
            .padding()
            .background(.thinMaterial, in: .rect(cornerRadius: 12))
    }
}
```

## Feedback and Haptics [Prefer]

- Prefer `sensoryFeedback` over manual UIKit or AppKit feedback generators when SwiftUI can express the interaction.
- Match feedback to a meaningful state change such as selection, success, warning, or error.
- Do not fire haptics on every transient update such as every scroll sample or keystroke.

```swift
Button("Save", action: save)
    .sensoryFeedback(.success, trigger: saveSucceeded)
```

## Previews and Sample Data [Prefer]

- Use `#Preview` when the toolchain supports it; keep `PreviewProvider` only for older toolchains or when the repo still standardizes on it.
- Preview reusable views in multiple states: loading, empty, error, long text, and large Dynamic Type where relevant.
- Inject sample data, sample environment values, and stub services; avoid network, database, or file-system side effects in previews.
- Keep preview scaffolding local to the component or feature so it does not leak into runtime code paths.
- For preview scope and fixture strategy, load `preview-strategies.md`.

```swift
#Preview("Loading") {
    ItemGrid(items: sampleItems)
}
```

## Component Checklist

- Typed tabs when selection matters
- Adaptive grids instead of screen-size constants
- Toolbars and menus for action density
- Placeholders that preserve layout
- Haptics tied to meaningful state changes
- Preview states cover realistic UI conditions
