# macOS Menu Bar and Settings

Read this file for menu bar utilities, preferences entry points, or lightweight macOS app shells.

- `[Prefer]` Use `MenuBarExtra` for persistent menu bar presence; Apple documents it as macOS 13+.
- `[Consider]` Use `.menuBarExtraStyle(.window)` when the extra needs richer SwiftUI content than a simple menu.
- `[Prefer]` Use a `Settings` scene for app preferences and `SettingsLink` or `openSettings` to open it when available; Apple documents both as macOS 14+.
- `[Only when]` Set `LSUIElement = true` for menu-bar-only apps that should hide their Dock icon.
- `[Only when]` Reach for AppKit status items only when `MenuBarExtra` cannot express the required behavior.
