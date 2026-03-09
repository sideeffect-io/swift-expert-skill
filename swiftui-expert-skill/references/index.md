# Reference Index

Use this file when the right reference is not obvious, or when the task is a broad review or assessment.

## Start by task

| If the task is... | Start here |
| --- | --- |
| broad code review or assessment | `api-modernization.md`, then `state-and-data-flow.md`, `view-composition.md`, `navigation-presentation-and-input.md`, `performance.md`, and `accessibility-and-design.md` |
| improve or refactor existing code | identify the dominant problem in the topic table below; if it is still unclear, start `api-modernization.md` |
| write a new screen or feature | start here, then load `state-and-data-flow.md`, `navigation-presentation-and-input.md`, and `component-patterns.md`; add `app-structure-and-deeplinks.md` for multi-screen, tab, scene, or deeplink flows |
| vague or mixed request | start `api-modernization.md`, then use the topic table below to narrow scope |

## Start by platform or release

| If the work is primarily... | Start here |
| --- | --- |
| iPhone, iPad, tvOS, watchOS, or visionOS SwiftUI UI work | `state-and-data-flow.md`, `navigation-presentation-and-input.md`, and `component-patterns.md` |
| macOS scenes, windows, settings, menu bar extras, tables, file panels, or AppKit interop | `macos-scenes-and-windows.md`, `macos-views-and-interop.md`, and the matching macOS micro references below |
| multiplatform app structure, tab-wide routing, scene-wide state, or deeplink handling | `app-structure-and-deeplinks.md`; add the macOS references when desktop scenes or AppKit interop matter |
| iOS 26 or macOS 26 Liquid Glass work, or new SwiftUI WebKit APIs | `liquid-glass.md` and `api-modernization.md` |

## Start by topic

| If the request mentions... | Start here |
| --- | --- |
| outdated APIs, migration, availability | `api-modernization.md` |
| state ownership, wrappers, bindings, `@Observable` | `state-and-data-flow.md` |
| large bodies, extraction, stable trees | `view-composition.md` |
| navigation, sheets, alerts, search, text input | `navigation-presentation-and-input.md` |
| tabs, toolbars, grids, previews, placeholders, haptics | `component-patterns.md` |
| app wiring, deeplinks, tab-wide or scene-wide routing | `app-structure-and-deeplinks.md` |
| lists, tables, scrolling, media, `ForEach` | `lists-scrolling-and-media.md` |
| jank, hangs, profiling, hot paths | `performance.md` |
| accessibility, typography, theming | `accessibility-and-design.md` |
| animations, hero transitions, navigation transitions | `animations-and-transitions.md` |
| `matchedTransitionSource`, `navigationTransition`, hero push transitions | `matched-and-navigation-transitions.md` |
| general Swift quality, concurrency, testing, localization, secrets, or hygiene | `swift-and-hygiene.md` |
| Liquid Glass | `liquid-glass.md` |
| macOS windows, settings, menu bar, AppKit interop | `macos-scenes-and-windows.md`, `macos-views-and-interop.md` |
| `AsyncImage`, thumbnails, downsampling | `asyncimage-and-downsampling.md` |
| sheet ownership, detents, popovers | `sheet-patterns.md` |
| programmatic scroll, `ScrollViewReader`, `scrollPosition` | `scrollview-and-position.md` |
| row identity and `ForEach` stability | `list-row-identity.md` |
| choosing `Toggle`, `Picker`, `Stepper`, `Slider` in forms | `form-controls.md` |
| loading states, `redacted`, `ContentUnavailableView` | `loading-placeholders.md` |
| macOS menu bar extras, `SettingsLink`, `openSettings` | `macos-menu-bar-and-settings.md` |
| macOS tables, `Table(sortOrder:)`, `fileImporter` | `macos-table-and-file-panels.md` |
| macOS scene-level window chrome | `macos-window-chrome.md` |
| search scopes or empty search states | `search-and-scopes.md` |
| keyboard focus or submit flow | `focus-and-keyboard.md` |
| sticky input bars or bottom actions | `input-bars-and-safe-area-insets.md` |
| title menus or dense toolbars | `title-menus-and-toolbars.md` |
| `overlay` vs `background` vs `ZStack` | `overlay-background-zstack.md` |
| preview design and sample data | `preview-strategies.md` |
| semantic tokens and theme propagation | `theme-tokens.md` |
