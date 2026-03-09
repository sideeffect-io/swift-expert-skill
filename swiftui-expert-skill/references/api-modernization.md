# API Modernization

Use Apple migration docs and API availability as the source of truth. Prefer newer APIs in new work, but do not force churn in stable legacy code unless the touched area benefits from the migration.

## In This File

- Availability snapshot
- Navigation and toolbar migrations
- Presentation and input migrations
- Styling and layout migrations
- Events and feedback migrations
- Environment and custom keys
- Web content
- Current migration summary

## Availability Snapshot

Availability refers to API availability from current Apple documentation, not your project's minimum deployment target.

| API | Availability | Guidance |
| --- | --- | --- |
| `NavigationStack` | iOS 16.0+, macOS 13.0+, tvOS 16.0+, watchOS 9.0+, visionOS 1.0+ | `[Prefer]` for new stack navigation |
| `NavigationSplitView` | iOS 16.0+, macOS 13.0+, tvOS 16.0+, watchOS 9.0+, visionOS 1.0+ | `[Prefer]` for multicolumn navigation |
| `onGeometryChange` | iOS 16.0+, macOS 13.0+, tvOS 16.0+, watchOS 9.0+, visionOS 1.0+ | `[Consider]` before `GeometryReader` when you only need derived geometry values |
| `AsyncImage` | iOS 15.0+, macOS 12.0+, tvOS 15.0+, watchOS 8.0+, visionOS 1.0+ | `[Prefer]` for straightforward remote image loading |
| `@Observable` | iOS 17.0+, macOS 14.0+, tvOS 17.0+, watchOS 10.0+, visionOS 1.0+ | `[Prefer]` for new observable models |
| `@Bindable` | iOS 17.0+, macOS 14.0+, tvOS 17.0+, watchOS 10.0+, visionOS 1.0+ | `[Prefer]` when injected observable models need writable bindings |
| `LabeledContent` | iOS 16.0+, macOS 13.0+, tvOS 16.0+, watchOS 9.0+, visionOS 1.0+ | `[Prefer]` for aligned labeled values and controls |
| `Tab("...", value: ...)` | iOS 18.0+, macOS 15.0+, tvOS 18.0+, watchOS 11.0+, visionOS 2.0+ | `[Prefer]` over `tabItem()` when available |
| `ScrollViewReader` | iOS 14.0+, macOS 11.0+, tvOS 14.0+, watchOS 7.0+, visionOS 1.0+ | `[Prefer]` for scroll-to-known-ID behavior |
| `scrollPosition(id:anchor:)` | iOS 17.0+, macOS 14.0+, tvOS 17.0+, watchOS 10.0+, visionOS 1.0+ | `[Consider]` when a view needs a bound visible item ID |
| `ScrollPosition` | iOS 18.0+, macOS 15.0+, tvOS 18.0+, watchOS 11.0+, visionOS 2.0+ | `[Consider]` for richer semantic scroll state and programmatic positioning |
| `presentationDetents` | iOS 16.0+, macOS 13.0+, tvOS 16.0+, watchOS 9.0+, visionOS 1.0+ | `[Consider]` when sheet height is part of the UX |
| `ContentUnavailableView` | iOS 17.0+, macOS 14.0+, tvOS 17.0+, watchOS 10.0+, visionOS 1.0+ | `[Prefer]` for empty and unavailable states on supported targets |
| `ContentUnavailableView.search` | iOS 17.0+, macOS 14.0+, tvOS 17.0+, watchOS 10.0+, visionOS 1.0+ | `[Prefer]` for empty search results inside searchable hierarchies |
| `Table(sortOrder:)` | iOS 16.0+, macOS 12.0+, visionOS 1.0+ | `[Prefer]` for sortable multi-column data |
| `fileImporter` | iOS 14.0+, macOS 11.0+, visionOS 1.0+ | `[Prefer]` for file import flows before custom panels |
| `MenuBarExtra` | macOS 13.0+ | `[Prefer]` for menu bar utilities and menu-bar-only apps |
| `SettingsLink` / `openSettings` | macOS 14.0+ | `[Prefer]` for opening a `Settings` scene from app UI |
| `navigationTransition` / `matchedTransitionSource` | iOS 18.0+, macOS 15.0+, tvOS 18.0+, watchOS 11.0+, visionOS 2.0+ | `[Prefer]` for hero-style navigation transitions |
| `UtilityWindow` | macOS 15.0+ | `[Only when]` the app needs a floating utility or inspector window |
| `glassEffect` | iOS 26.0+, macOS 26.0+, tvOS 26.0+, watchOS 26.0+ | `[Only when]` Liquid Glass is explicitly requested |
| `WebView` | iOS 26.0+, macOS 26.0+, visionOS 26.0+ | `[Prefer]` over wrapped `WKWebView` when the target supports it |

## Navigation and Toolbar [Deprecated + Prefer]

- Prefer `NavigationStack` and `NavigationSplitView` for new code. `NavigationView` still exists, but Apple documents migration paths to the newer navigation types.
- Prefer `navigationDestination(for:)` with `Hashable` route values over destination-based `NavigationLink` when building typed routing.
- Do not mix value-driven and destination-driven navigation in the same hierarchy unless the existing structure already depends on it.
- Prefer `navigationTitle(_:)` over `navigationBarTitle(_:)`.
- Prefer `toolbar {}` over `navigationBarItems(...)`.
- Use `.topBarLeading` and `.topBarTrailing` instead of deprecated navigation-bar placements.
- Prefer the modern `Tab` API over `tabItem()` when the deployment target supports it.

```swift
enum Route: Hashable {
    case detail(Item.ID)
    case settings
}

NavigationStack {
    List(items) { item in
        NavigationLink(item.name, value: Route.detail(item.id))
    }
    .navigationTitle("Items")
    .navigationDestination(for: Route.self) { route in
        switch route {
        case .detail(let id):
            DetailView(id: id)
        case .settings:
            SettingsView()
        }
    }
}
```

## Presentation and Input [Deprecated + Prefer]

- Prefer `confirmationDialog` over `actionSheet`.
- Prefer the modern `alert(_:isPresented:actions:message:)` overloads over legacy `Alert(...)` builders.
- Prefer `textInputAutocapitalization(_:)` over `autocapitalization(_:)`.
- Prefer `focused(...)` and `onSubmit` over legacy `TextField` editing callbacks.
- Use `sheet(item:)` when the presented state is naturally optional model data.

## Styling and Layout [Prefer]

- Prefer `foregroundStyle` when you want semantic colors, materials, or multi-level styles. `foregroundColor(_:)` is still valid, so treat this as a preference, not a deprecation.
- Prefer `clipShape(.rect(cornerRadius:))` in new code instead of `cornerRadius(_:)`.
- Prefer dedicated accessibility modifiers such as `accessibilityLabel` and `accessibilityValue` over generic `accessibility(...)` overloads.
- Prefer the builder-style `overlay(alignment:content:)` and `background(alignment:content:)` when clarity or multiple overlay views matter. The older `overlay(_:alignment:)` and `background(_:alignment:)` overloads are still valid.
- Prefer `ignoresSafeArea` over `edgesIgnoringSafeArea`.
- Prefer `preferredColorScheme(_:)` over the deprecated `colorScheme(_:)` modifier.
- Prefer `containerRelativeFrame`, `visualEffect`, `onGeometryChange`, or custom `Layout` types before reaching for `GeometryReader` in new code.
- Prefer `scrollIndicators(.hidden)` over older initializer flags when changing indicator visibility after construction.
- Prefer text interpolation over `Text` concatenation with `+`.

```swift
Text("Status")
    .foregroundStyle(.primary)
    .padding(12)
    .overlay(alignment: .trailing) {
        Image(systemName: "checkmark.circle.fill")
            .foregroundStyle(.green)
    }
```

## Events and Feedback [Prefer]

- Prefer `.animation(_:value:)` or scoped animation blocks over `animation(_:)` without a tracked value.
- The older `onChange(of:perform:)` overload is still valid. Prefer iOS 17+ `onChange(of:initial:_:)` when you need initial firing or cleaner captured-value behavior.
- Prefer `sensoryFeedback` over manually bridging to UIKit/AppKit haptic generators when SwiftUI supports the interaction.

```swift
.onChange(of: query, initial: false) {
    runSearch()
}
```

## Environment and Custom Keys [Consider]

- Prefer the `@Entry` macro for custom `EnvironmentValues`, `FocusValues`, `Transaction`, or container keys when the project uses current toolchains that support it.
- Keep manual key types only when the codebase must support older toolchains or already uses the legacy pattern consistently.

## Web Content [Only when]

- On iOS 26+, macOS 26+, and related current SDKs, prefer WebKit for SwiftUI with `WebView` and `WebPage` instead of wrapping `WKWebView` yourself.
- On older deployment targets, keep `UIViewRepresentable` or `NSViewRepresentable` wrappers for WebKit.

```swift
import SwiftUI
import WebKit

struct BrowserView: View {
    @State private var page = WebPage()

    var body: some View {
        NavigationStack {
            WebView(page)
                .navigationTitle(page.title)
        }
    }
}
```

## Current Migration Summary

- Treat Apple-documented deprecations as hard migrations.
- Treat stylistic source rules as preferences unless Apple actually deprecated the older API.
- Prefer the newest API that matches the project deployment target and surrounding code style.
