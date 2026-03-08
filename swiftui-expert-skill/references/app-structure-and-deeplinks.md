# App Structure and Deeplinks

Read this file when the request spans multiple screens, tabs, app entry points, scenes, or deep-link handling.

Keep the guidance architecture agnostic: describe ownership, routing, and presentation responsibilities without forcing MVVM, TCA, coordinators, or any specific folder layout.

## Root Composition

- Keep the `App` entry point thin. Compose scenes, inject shared services or observable models, and hand off to feature roots quickly.
- Decide which state is app-wide, scene-wide, feature-wide, or purely local before creating routing or presentation state.
- Inject shared dependencies at scene or feature boundaries rather than reaching for global singletons from leaf views.

```swift
@main
struct ExampleApp: App {
    @State private var session = SessionModel()

    var body: some Scene {
        WindowGroup {
            RootView()
                .environment(session)
        }
    }
}
```

## Tabs, Navigation, and Sheets

- Model tab selection with a typed enum.
- Model navigation with typed route values or a typed `NavigationPath` wrapper when heterogenous paths are truly required.
- Keep app-wide sheets or alerts centralized only when they genuinely cross feature boundaries. Otherwise keep presentation local to the owning screen.
- If the UX expects each tab to preserve its own stack, store one path per tab.

```swift
enum AppTab: Hashable {
    case home
    case inbox
    case settings
}

enum AppSheet: String, Identifiable {
    case compose
    case account

    var id: String { rawValue }
}
```

## Typed Routes Over Stringly Routing

- Prefer enums or strongly typed values for routes, sheets, and modal destinations.
- Avoid scattering raw path strings, URL fragments, or boolean presentation flags through many leaf views.
- Keep conversion from external inputs such as URLs or notifications to internal route types in one place.

```swift
enum Route: Hashable {
    case article(id: UUID)
    case profile(username: String)
}
```

## Deeplink Flow

- Parse the incoming URL or user activity once.
- Convert it into typed app intent: tab selection, pushed route, presented sheet, or a combination.
- Switch tab or scene first, then update the navigation path or presented sheet.
- Make deeplink handling idempotent so repeated delivery does not duplicate pushes or present the same sheet twice.
- If a deeplink requires data fetch before presentation, represent that loading step explicitly rather than pushing incomplete placeholder navigation state blindly.

```swift
func handle(url: URL) {
    guard let intent = AppIntent(url: url) else { return }

    selectedTab = intent.tab

    switch intent.destination {
    case .article(let id):
        homePath.append(Route.article(id: id))
    case .profile(let username):
        presentedSheet = .account
        pendingProfile = username
    }
}
```

## URL and Activity Entry Points

- Use `onOpenURL` for universal links, custom schemes, and in-app routing hooks.
- Use `onContinueUserActivity` when the platform delivers handoff, Spotlight, or related activity payloads.
- Keep the parsing layer testable by separating raw URL parsing from SwiftUI view code.

## Multi-Scene and Multiplatform Notes

- If the flow opens separate windows, settings, menu bar extras, or utility panels on macOS, also load `macos-scenes-and-windows.md`.
- If the destination uses `Table`, file panels, or AppKit interop, also load `macos-views-and-interop.md`.
- Prefer scene-specific state when multiple windows can navigate independently.

## External Events and Restoration

- When the app can be launched into multiple scenes or windows, route the external event to the correct scene rather than assuming one shared navigation stack.
- Restore tab selection, navigation path, and transient sheet state only when that restoration materially improves UX and does not re-trigger destructive flows.

## App-Level Checklist

- Thin `App` entry point
- Clear ownership boundary for app-wide vs local state
- Typed tabs, routes, and sheets
- Centralized deeplink parsing
- Idempotent external-event handling
- Scene-aware routing on macOS or multiwindow apps
