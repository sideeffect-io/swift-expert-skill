---
name: swiftui-expert
description: Write, review, refactor, or modernize SwiftUI across Apple platforms using current APIs, Observation-based data flow, strong view composition, navigation and presentation patterns, performance, accessibility, animations, macOS scenes and views, and optional iOS 26 Liquid Glass.
---

# SwiftUI Expert

Use this skill for SwiftUI code generation, refactors, reviews, and API modernization. Prefer native SwiftUI and Apple guidance. Keep recommendations architecture-agnostic unless the existing codebase already commits to a specific pattern.

## Workflow

### 1. Review existing code

- Start with `references/api-modernization.md` for current API choices and migration wording.
- Load only the references that match the problem:
  - state or wrappers: `references/state-and-data-flow.md`
  - body size, identity, or extraction: `references/view-composition.md`
  - navigation, forms, sheets, search, or input: `references/navigation-presentation-and-input.md`
  - lists, scrolling, media, or `ForEach`: `references/lists-scrolling-and-media.md`
  - runtime cost, jank, hangs, or excessive updates: `references/performance.md`
  - accessibility or design polish: `references/accessibility-and-design.md`
  - animations or transitions: `references/animations-and-transitions.md`
  - iOS 26 glass work: `references/liquid-glass.md`
  - macOS-specific APIs or multiplatform behavior: `references/macos-scenes-and-windows.md` and `references/macos-views-and-interop.md`

### 2. Improve or refactor code

- Keep behavior stable unless the task explicitly asks for product changes.
- Prefer the smallest change that improves correctness, clarity, or performance.
- Rewrite overclaims as preferences: use `prefer` or `consider` when Apple has not deprecated an API.
- Remove repo-specific scaffolding, router, or theme-singleton assumptions from any borrowed pattern.

### 3. Implement new UI

- Decide data ownership first, then wrappers, then navigation and presentation.
- Keep the view tree stable, use small focused subviews, and keep side effects out of `body`.
- Choose system containers first: `List`, `Form`, `NavigationStack`, `NavigationSplitView`, `Table`, `MenuBarExtra`, `Settings`, and native controls before custom replacements.
- Gate platform- or version-specific features with availability checks and provide fallbacks when needed.

## Core Guidance

- Prefer modern Observation for new code, but support legacy `ObservableObject` code when a migration would be out of scope.
- Prefer `Button`, `Menu`, `Toggle`, `Label`, `ContentUnavailableView`, and other semantic controls over gesture-only or purely custom equivalents.
- Prefer typed navigation and model-driven presentation over loosely coupled booleans and destination closures.
- Prefer stable identity and constant row structure in dynamic collections.
- Prefer moving heavy work out of `body`, scroll handlers, and other hot paths.
- Prefer Apple-backed API migrations over style churn.
- Prefer explicit availability notes for iOS 26 Liquid Glass and new WebKit SwiftUI APIs.
- Read the macOS references only when the task genuinely includes macOS scenes, windows, tables, split views, or AppKit interop.

## Quick Review Checklist

- [ ] APIs are current for the deployment target, without inventing deprecations.
- [ ] State ownership and wrapper choice match the data flow.
- [ ] `body` stays readable, stable, and free of side effects.
- [ ] Navigation and presentation use a consistent modern pattern.
- [ ] Lists, tables, and `ForEach` use stable identity and avoid hot-path work.
- [ ] Accessibility, Dynamic Type, and hit targets are preserved.
- [ ] Animations and transitions use the right scope and context.
- [ ] Performance fixes are evidence-based; profile when code review is not enough.
- [ ] iOS 26 glass code is opt-in, availability-gated, and paired with a fallback.
- [ ] macOS-only guidance is loaded only when the target actually needs it.
