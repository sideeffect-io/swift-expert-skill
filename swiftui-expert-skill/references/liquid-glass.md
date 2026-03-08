# Liquid Glass

Read this file only when the task explicitly targets iOS 26+ design work or asks for Liquid Glass.

## Default Stance

- Treat Liquid Glass as opt-in, not a blanket restyling pass.
- Prefer system glass that already comes from toolbars, controls, and platform UI before adding custom glass surfaces.
- Always gate custom glass code with availability checks and provide a non-glass fallback.

```swift
if #available(iOS 26, *) {
    content
        .padding()
        .glassEffect(.regular.interactive(), in: .rect(cornerRadius: 16))
} else {
    content
        .padding()
        .background(.ultraThinMaterial, in: RoundedRectangle(cornerRadius: 16))
}
```

## Core Rules

- Use `glassEffect` for custom surfaces.
- Use `GlassEffectContainer` when multiple nearby glass elements should render and morph as a group.
- Match `GlassEffectContainer` spacing to the actual layout spacing.
- Apply `glassEffect` after layout and other appearance modifiers.
- Use `.interactive()` only on elements that really respond to input.
- Keep shapes, corner radii, and tint strategy consistent across related elements.

```swift
if #available(iOS 26, *) {
    GlassEffectContainer(spacing: 16) {
        HStack(spacing: 16) {
            Button("Edit") {}
                .buttonStyle(.glass)
            Button("Save") {}
                .buttonStyle(.glassProminent)
        }
    }
}
```

## Morphing

- Use `glassEffectID` with a shared `@Namespace` for morphing transitions.
- The morphing views need matching IDs, the same namespace, and a shared container.

```swift
@Namespace private var glassNamespace
```

## Review Checklist

- Availability-gated
- Fallback present
- Modifier order correct
- Interactive only when appropriate
- Grouped glass wrapped in `GlassEffectContainer`
