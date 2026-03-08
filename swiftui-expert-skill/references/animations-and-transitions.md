# Animations and Transitions

Animate the state change that matters, at the narrowest useful scope.

## Choosing the Tool [Prefer]

- Use `.animation(_:value:)` for view-local value changes.
- Use `withAnimation` for event-driven mutations.
- Use transitions when a view enters or leaves the tree.
- Use `phaseAnimator` or `keyframeAnimator` for multi-step choreography on supported OS versions.

```swift
Button("Toggle") {
    withAnimation(.spring) {
        isExpanded.toggle()
    }
}
```

## Placement and Scope [Prefer]

- Place the animation as close as possible to the view that should animate.
- Avoid putting animation high in the tree unless the whole subtree should animate.
- Prefer transforms such as `offset`, `scaleEffect`, and `rotationEffect` over expensive layout changes when the visual result allows it.

## Transitions [Prefer]

- A transition needs animation context outside the conditional branch.
- Use asymmetric transitions when insertion and removal should feel different.
- Do not expect a transition to run if you attach animation only inside the disappearing branch.

```swift
if showInspector {
    InspectorView()
        .transition(.move(edge: .trailing).combined(with: .opacity))
}
```

## Navigation and Hero Transitions [Prefer, iOS 18+ / macOS 15+]

- Use `matchedTransitionSource(id:in:)` on the source view and `navigationTransition(...)` on the destination when you want a hero-style navigation effect.
- Prefer this for list, grid, or card-to-detail transitions where the source and destination identity is stable.
- Keep the source ID stable and scoped to a shared namespace.
- Fall back to the default navigation transition when the deployment target is earlier or when the source and destination do not map cleanly.
- For a compact decision summary, load `matched-and-navigation-transitions.md`.

```swift
@Namespace private var heroNamespace

NavigationLink(value: item) {
    ItemCard(item: item)
        .matchedTransitionSource(id: item.id, in: heroNamespace)
}
.navigationDestination(for: Item.self) { item in
    DetailView(item: item)
        .navigationTransition(.zoom(sourceID: item.id, in: heroNamespace))
}
```

## Custom Animatable Types [Consider]

- If you implement `Animatable` manually, define `animatableData` explicitly.
- Prefer the `@Animatable` macro when the toolchain supports it; exclude non-animatable properties with `@AnimatableIgnored`.

```swift
@Animatable
struct BadgeShape: Shape {
    var amount: CGFloat
    @AnimatableIgnored var isHighlighted = false

    func path(in rect: CGRect) -> Path {
        Path(ellipseIn: rect.insetBy(dx: amount, dy: amount))
    }
}
```

## Completion and Chaining [Consider]

- Chain animations with completion handlers, phases, or keyframes instead of hard-coded delays whenever possible.
- Avoid animating every scroll tick or every geometry sample.

## Debugging [Consider]

- Slow the animation down in debug builds when needed.
- Use `Self._logChanges()` or `Self._printChanges()` if unexpected view updates are causing animations to restart.
