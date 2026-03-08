# Accessibility and Design

Accessibility is not a polish pass. Build it into control choice, layout, motion, and semantics.

## Semantics First

- Prefer `Button` over `onTapGesture` for tappable UI.
- Prefer `Label` over ad hoc `HStack { Image; Text }` when the UI is conceptually one labeled control.
- Give image-based buttons a real text label even if the text is visually hidden.
- If a gesture-only interaction is unavoidable, add the correct accessibility traits.

```swift
Button("Add Item", systemImage: "plus", action: addItem)
```

## Dynamic Type and Hit Targets

- Let system text styles scale naturally.
- Use `@ScaledMetric` for custom spacing, icon sizes, or padding that should scale with type.
- Keep interactive targets at or above roughly 44x44 points on iOS unless platform conventions say otherwise.

```swift
@ScaledMetric(relativeTo: .body) private var avatarSize = 56.0
```

## Grouping and Custom Controls

- Use `accessibilityElement(children: .combine)` for one logical value made from multiple visual pieces.
- Use `.ignore` when you need a custom spoken label for the group.
- Use `.contain` for grouped controls such as a tab strip.
- Use `accessibilityRepresentation` when a custom-drawn control should behave like a standard SwiftUI control.
- Use `accessibilityAdjustableAction` for increment/decrement controls.

## Respect User Settings

- Respect Reduce Motion. Replace large motion with subtler opacity or scale changes when appropriate.
- Respect Differentiate Without Color. Do not rely on color alone to convey state.
- Prefer semantic foreground and background styles over manual opacity tricks when system styles can express the hierarchy.

## System Design Defaults

- Prefer `ContentUnavailableView` for empty states.
- Prefer `LabeledContent` inside `Form` rows.
- Avoid hard-coded screen metrics such as `UIScreen.main.bounds`; prefer SwiftUI layout tools.
- Avoid fixed frames when the content should adapt across device sizes or Dynamic Type settings.
- Prefer semantic `Color` and asset colors over UIKit/AppKit color literals in SwiftUI views.

## Text and Typography

- Prefer `bold()` for bold emphasis; use `fontWeight` only when you need a specific non-bold weight.
- Use small text styles like `.caption2` sparingly.
- Prefer localization-friendly APIs such as text interpolation and inflection over brittle concatenation.
