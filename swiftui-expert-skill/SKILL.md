---
name: swiftui-expert
description: Write, review, assess, refactor, or modernize SwiftUI across Apple platforms using current APIs, Observation-based data flow, view composition, navigation and presentation patterns, tabs and forms, deeplinks, performance and profiling, accessibility, theming, previews, animations, macOS scenes and views, Swift and concurrency hygiene, and optional iOS 26 Liquid Glass.
---

# SwiftUI Expert

Use this skill for SwiftUI code generation, assessment, refactors, reviews, and API modernization. Prefer native SwiftUI and Apple guidance. Keep recommendations architecture-agnostic unless the existing codebase already commits to a specific pattern.

## Reference Tags

- `[Deprecated]`: Apple-documented deprecation or migration path.
- `[Prefer]`: strong default for new code.
- `[Consider]`: context-dependent heuristic.
- `[Only when]`: narrow or feature-specific use.

## Workflow

### 0. Route by task

- For broad code review or assessment with no obvious topic, start with `references/api-modernization.md`, then load `references/state-and-data-flow.md`, `references/view-composition.md`, `references/navigation-presentation-and-input.md`, `references/performance.md`, and `references/accessibility-and-design.md` as the code warrants.
- For improving existing code, identify whether the dominant issue is state, composition, navigation, app wiring, collection behavior, performance, accessibility, animation, or surrounding Swift hygiene, then open the matching reference from `references/index.md`.
- For new screens or features, start with `references/index.md`, then load `references/state-and-data-flow.md`, `references/navigation-presentation-and-input.md`, and `references/component-patterns.md` as the baseline set. Add `references/app-structure-and-deeplinks.md` when the feature spans tabs, scenes, or external entry points.
- For vague or mixed prompts, start with `references/index.md` and load only the files that match the dominant risk.

### 1. Review or assess existing code

- Start with `references/api-modernization.md` for current API choices and migration wording.
- If the right file is unclear, start with `references/index.md`. It routes to the main references and the focused micro references: `references/asyncimage-and-downsampling.md`, `references/sheet-patterns.md`, `references/scrollview-and-position.md`, `references/list-row-identity.md`, `references/form-controls.md`, `references/loading-placeholders.md`, `references/macos-menu-bar-and-settings.md`, `references/macos-table-and-file-panels.md`, `references/macos-window-chrome.md`, `references/search-and-scopes.md`, `references/focus-and-keyboard.md`, `references/matched-and-navigation-transitions.md`, `references/input-bars-and-safe-area-insets.md`, `references/title-menus-and-toolbars.md`, `references/overlay-background-zstack.md`, `references/preview-strategies.md`, and `references/theme-tokens.md`.
- Load only the references that match the problem:
  - state or wrappers: `references/state-and-data-flow.md`
  - body size, identity, or extraction: `references/view-composition.md`
  - navigation, forms, sheets, search, or input: `references/navigation-presentation-and-input.md`
  - tabs, grids, toolbars, placeholders, haptics, or previews: `references/component-patterns.md`
  - app entry points, tab wiring, routes, or deeplinks: `references/app-structure-and-deeplinks.md`
  - lists, scrolling, media, or `ForEach`: `references/lists-scrolling-and-media.md`
  - runtime cost, jank, hangs, or excessive updates: `references/performance.md`
  - accessibility, design, or theming: `references/accessibility-and-design.md`
  - animations, hero transitions, or navigation transitions: `references/animations-and-transitions.md`
  - general Swift quality, concurrency, testing, localization, or secrets: `references/swift-and-hygiene.md`
  - iOS 26 glass work: `references/liquid-glass.md`
  - macOS-specific APIs or multiplatform behavior: `references/macos-scenes-and-windows.md` and `references/macos-views-and-interop.md`

### 2. Improve or refactor code

- Keep behavior stable unless the task explicitly asks for product changes.
- Prefer the smallest change that improves correctness, clarity, or performance.
- Rewrite overclaims as preferences: use `prefer` or `consider` when Apple has not deprecated an API.
- Remove repo-specific scaffolding, router, or theme-singleton assumptions from any borrowed pattern.

### 3. Implement new UI

- Decide data ownership first, then wrappers, then navigation and presentation.
- Decide whether the feature is local, tab-wide, scene-wide, or app-wide before inventing route or presentation state.
- Keep the view tree stable, use small focused subviews, and keep side effects out of `body`.
- Choose system containers first: `List`, `Form`, `NavigationStack`, `NavigationSplitView`, `Table`, `MenuBarExtra`, `Settings`, and native controls before custom replacements.
- Prefer typed tabs, routes, sheets, and deeplink intents over stringly or boolean-heavy wiring when a flow spans multiple screens.
- Gate platform- or version-specific features with availability checks and provide fallbacks when needed.

## Core Guidance

- Prefer modern Observation for new code, but support legacy `ObservableObject` code when a migration would be out of scope.
- Prefer `Button`, `Menu`, `Toggle`, `Label`, `ContentUnavailableView`, and other semantic controls over gesture-only or purely custom equivalents.
- Prefer typed navigation and model-driven presentation over loosely coupled booleans and destination closures.
- Prefer enum-backed `TabView` selection and typed deeplink intents when navigation spans multiple features.
- Prefer stable identity and constant row structure in dynamic collections.
- Prefer moving heavy work out of `body`, scroll handlers, and other hot paths.
- Prefer semantic theming and preview-friendly component design over ad hoc styling or preview-only hacks.
- Prefer structured concurrency, actor-safe UI state, and locale-aware formatting in surrounding Swift code.
- Prefer Apple-backed API migrations over style churn.
- Prefer explicit availability notes for iOS 26 Liquid Glass and new WebKit SwiftUI APIs.
- Read the macOS references only when the task genuinely includes macOS scenes, windows, tables, split views, or AppKit interop.

## Review Heuristics

- Start with the smallest owner of the state and the narrowest owner of the navigation.
- Separate Apple deprecations from stylistic preferences.
- Prefer semantic controls, semantic styling, and stable identity before clever abstraction.
- Treat performance advice as a hypothesis until code review or profiling supports it.
- Load micro references for long-tail UI patterns instead of inflating the main files.

## Quick Review Checklist

- [ ] APIs are current for the deployment target, without inventing deprecations.
- [ ] State ownership and wrapper choice match the data flow.
- [ ] `body` stays readable, stable, and free of side effects.
- [ ] Navigation and presentation use a consistent modern pattern.
- [ ] Tabs, app-level routes, and deeplinks are typed when the feature spans multiple screens.
- [ ] Lists, tables, and `ForEach` use stable identity and avoid hot-path work.
- [ ] Forms, focus, toolbars, placeholders, and haptics use native SwiftUI patterns where appropriate.
- [ ] Accessibility, Dynamic Type, and hit targets are preserved.
- [ ] Animations and transitions use the right scope and context.
- [ ] Performance fixes are evidence-based; profile with Instruments when code review is not enough.
- [ ] General Swift code around the view layer uses structured concurrency, safe persistence, and project hygiene.
- [ ] iOS 26 glass code is opt-in, availability-gated, and paired with a fallback.
- [ ] macOS-only guidance is loaded only when the target actually needs it.
