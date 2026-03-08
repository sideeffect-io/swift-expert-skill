# View Composition

SwiftUI diffs view trees. Stable identity and small focused subviews usually beat clever indirection.

## Keep `body` Simple

- Keep `body` declarative and side-effect free.
- Move imperative work into methods, models, tasks, or view modifiers.
- Extract complex sections into subviews when they have their own responsibility, expensive rendering, or isolated inputs.

```swift
struct DetailView: View {
    let item: Item

    var body: some View {
        VStack(spacing: 16) {
            HeaderView(item: item)
            MetadataView(item: item)
            ActionsView(item: item)
        }
    }
}
```

## Prefer Stable Trees

- Use modifiers or value changes when you are expressing two states of the same view.
- Use conditionals when the branches are genuinely different views or optional content.
- Avoid top-level `if` branches or helper extensions that change the view type for small stylistic changes.

```swift
Text(item.title)
    .foregroundStyle(item.isWarning ? .orange : .primary)
    .opacity(item.isHidden ? 0 : 1)
```

## Extraction Guidelines

- Prefer extracted `View` types over large computed view builders for complex sections.
- Small local helpers are fine when they are obviously cheap and improve readability.
- Keep action closures and business rules out of layout-heavy code.

```swift
struct RowList: View {
    let items: [Item]

    var body: some View {
        ForEach(items) { item in
            ItemRow(item: item)
        }
    }
}
```

## Container Pattern

- For reusable containers, prefer storing built view content with `@ViewBuilder let content: Content` instead of escaping content closures.
- This keeps the container easier for SwiftUI to diff and keeps the synthesized initializer ergonomic.

```swift
struct Card<Content: View>: View {
    @ViewBuilder let content: Content

    var body: some View {
        VStack(alignment: .leading, spacing: 12) {
            content
        }
        .padding()
        .background(.thinMaterial, in: .rect(cornerRadius: 16))
    }
}
```

## `overlay` / `background` vs `ZStack`

- Use `overlay` or `background` when decorating a primary view.
- Use `ZStack` when multiple peers jointly define layout.
- Prefer the modifier form when the modified view should remain the layout anchor.

```swift
Button("Continue", action: submit)
    .overlay(alignment: .trailing) {
        Image(systemName: "arrow.right.circle.fill")
    }
```

## Represent State Honestly

- If a piece of UI exists in both states, change its values, not its identity.
- If the content is optional, truly alternate, or backed by different data, a conditional is fine.
- Do not force everything into a single tree if it makes the code lie about what is actually on screen.

## Reuse

- Prefer `ViewModifier`, `ButtonStyle`, or dedicated small views for shared styling and behavior.
- Do not hide major structure behind generic helper extensions when a named view would be clearer.
