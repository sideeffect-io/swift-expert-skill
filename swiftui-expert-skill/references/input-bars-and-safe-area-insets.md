# Input Bars and Safe Area Insets

Read this file for sticky compose bars, bottom actions, or accessory chrome tied to scrolling content.

- `[Prefer]` Use `safeAreaInset(edge:)` for bottom input bars and persistent actions anchored to the viewport.
- `[Prefer]` Let the scrolling content remain the primary layout; add the bar as chrome, not as an extra row in the scroll content.
- `[Prefer]` Keep inset content lightweight and self-contained.
- `[Consider]` Pair the inset with materials or separators when it should read as independent chrome.
- `[Only when]` Use overlay-based positioning only when the inset must intentionally ignore safe-area behavior or interact with custom transitions.
