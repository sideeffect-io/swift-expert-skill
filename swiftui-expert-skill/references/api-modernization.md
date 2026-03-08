# API Modernization

Use Apple migration docs and API availability as the source of truth. Prefer newer APIs in new work, but do not force churn in stable legacy code unless the touched area benefits from the migration.

## Navigation and Toolbar

- Prefer `NavigationStack` and `NavigationSplitView` for new code. `NavigationView` still exists, but Apple documents migration paths to the newer navigation types.
- Prefer `navigationDestination(for:)` with `Hashable` route values over destination-based `NavigationLink` when building typed routing.
- Do not mix value-driven and destination-driven navigation in the same hierarchy unless the existing structure already depends on it.
- Prefer `navigationTitle(_:)` over `navigationBarTitle(_:)`.
- Prefer `toolbar {}` over `navigationBarItems(...)`.
- Use `.topBarLeading` and `.topBarTrailing` instead of deprecated navigation-bar placements.

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

## Presentation and Input

- Prefer `confirmationDialog` over `actionSheet`.
- Prefer the modern `alert(_:isPresented:actions:message:)` overloads over legacy `Alert(...)` builders.
- Prefer `textInputAutocapitalization(_:)` over `autocapitalization(_:)`.
- Prefer `focused(...)` and `onSubmit` over legacy `TextField` editing callbacks.
- Use `sheet(item:)` when the presented state is naturally optional model data.

## Styling and Layout

- Prefer `foregroundStyle` when you want semantic colors, materials, or multi-level styles. `foregroundColor(_:)` is still valid, so treat this as a preference, not a deprecation.
- Prefer `clipShape(.rect(cornerRadius:))` in new code instead of `cornerRadius(_:)`.
- Prefer dedicated accessibility modifiers such as `accessibilityLabel` and `accessibilityValue` over generic `accessibility(...)` overloads.
- Prefer the builder-style `overlay(alignment:content:)` and `background(alignment:content:)` when clarity or multiple overlay views matter. The older `overlay(_:alignment:)` and `background(_:alignment:)` overloads are still valid.
- Prefer `ignoresSafeArea` over `edgesIgnoringSafeArea`.
- Prefer `preferredColorScheme(_:)` over the deprecated `colorScheme(_:)` modifier.

```swift
Text("Status")
    .foregroundStyle(.primary)
    .padding(12)
    .overlay(alignment: .trailing) {
        Image(systemName: "checkmark.circle.fill")
            .foregroundStyle(.green)
    }
```

## Events and Feedback

- Prefer `.animation(_:value:)` or scoped animation blocks over `animation(_:)` without a tracked value.
- The older `onChange(of:perform:)` overload is still valid. Prefer iOS 17+ `onChange(of:initial:_:)` when you need initial firing or cleaner captured-value behavior.
- Prefer `sensoryFeedback` over manually bridging to UIKit/AppKit haptic generators when SwiftUI supports the interaction.

```swift
.onChange(of: query, initial: false) {
    runSearch()
}
```

## Environment and Custom Keys

- Prefer the `@Entry` macro for custom `EnvironmentValues`, `FocusValues`, `Transaction`, or container keys when the project uses current toolchains that support it.
- Keep manual key types only when the codebase must support older toolchains or already uses the legacy pattern consistently.

## Web Content

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
