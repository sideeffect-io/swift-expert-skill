# macOS Scenes and Windows

Read this file only when the task targets macOS-specific APIs or multiplatform window behavior.

## Scene Types

- Use `Settings` for app preferences on macOS. Gate it with `#if os(macOS)` in multiplatform apps.
- Use `MenuBarExtra` for menu bar utilities instead of reaching for `NSStatusItem` first.
- Use `WindowGroup` when multiple windows of the same kind may exist.
- Use `Window` for a singleton supplementary window.
- Use `UtilityWindow` for floating inspectors or palettes tied to the focused main window.

```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }

        #if os(macOS)
        Settings {
            SettingsView()
        }
        #endif
    }
}
```

## Settings

- `Settings` is macOS-only.
- Use `SettingsLink` or `openSettings` to open the settings window from app UI.
- Tabbed settings views are a good default when there are multiple groups of preferences.

```swift
struct SidebarFooter: View {
    var body: some View {
        SettingsLink {
            Label("Preferences", systemImage: "gear")
        }
    }
}
```

## Menu Bar Extras

- `MenuBarExtra` supports `.menu` and `.window` styles.
- Use `.window` for richer SwiftUI content.
- For menu-bar-only apps, set `LSUIElement = true` if the Dock icon should be hidden.

```swift
MenuBarExtra("Status", systemImage: "chart.bar") {
    StatusView()
}
.menuBarExtraStyle(.window)
```

## Window Configuration

- Use `defaultSize`, `defaultPosition`, and `windowResizability` to define default macOS window behavior.
- Use `.windowToolbarStyle(...)` and `.windowStyle(...)` at the scene level.
- Prefer `NavigationSplitView` for sidebar-driven navigation.
- Use `Inspector` for trailing detail panels on supported OS versions.

```swift
WindowGroup {
    RootView()
        .frame(minWidth: 700, minHeight: 480)
}
.defaultSize(width: 960, height: 640)
.windowResizability(.contentMinSize)
.windowToolbarStyle(.unified)
```

## `WindowGroup` vs `Window` vs `UtilityWindow`

- `WindowGroup`: many instances, normal app lifetime.
- `Window`: one instance, good for singleton tools or dashboards.
- `UtilityWindow`: floating secondary utility tied to the focused main scene; good for inspectors and palettes.

## Navigation Split View on macOS

- Use `NavigationSplitView` for sidebar/content/detail flows.
- Configure widths with `navigationSplitViewColumnWidth`.
- Remember that compact environments can collapse columns into a stack on other platforms, so build detail flows that still make sense when collapsed.
