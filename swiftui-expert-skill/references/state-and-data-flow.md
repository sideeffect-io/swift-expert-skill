# State and Data Flow

Choose ownership first, then choose the wrapper that matches it.

## Default Model for New Code

- Prefer `@Observable` models on iOS 17+, macOS 14+, and equivalent platform releases.
- Store an owned observable reference in `@State` so SwiftUI preserves the instance across view refreshes.
- Inject shared observable models with `@Environment`.
- Use `@Bindable` only when an injected observable needs writable bindings.
- Mark UI-bound observable reference types `@MainActor` unless the project already uses main-actor default isolation.

```swift
@Observable
@MainActor
final class SessionModel {
    var name = ""
    var isSaving = false
}

struct ProfileView: View {
    @State private var model = SessionModel()

    var body: some View {
        Form {
            TextField("Name", text: $model.name)
        }
    }
}
```

## Wrapper Selection

| Use | Wrapper |
| --- | --- |
| Internal value state owned by the view | `@State private var` |
| Child must mutate parent-owned value | `@Binding` |
| Injected observable needs bindings | `@Bindable` |
| Read-only input from parent | `let` |
| Single focusable field | `@FocusState Bool` |
| Multiple focusable fields | `@FocusState Field?` |
| Legacy owned `ObservableObject` | `@StateObject` |
| Legacy injected `ObservableObject` | `@ObservedObject` |

```swift
struct EditorView: View {
    @Bindable var model: SessionModel

    var body: some View {
        TextField("Name", text: $model.name)
    }
}
```

## Practical Rules

- Mark `@State` private.
- Use `@Binding` for downward mutation, not for read-only display.
- Prefer `let` for read-only child inputs. Use `var` only when the child needs `.onChange` or another reactive read.
- Avoid creating `ObservableObject` instances inline with `@ObservedObject`.
- Keep shared state narrow. Do not pass entire app models to leaf views if they only need one value.

## Property Wrappers Inside `@Observable`

Property wrappers and Observation macros can interact in surprising ways.

- Prefer keeping `@AppStorage`, `@SceneStorage`, and similar wrapper-backed persistence in views or dedicated storage helpers rather than inside `@Observable` models.
- If a property inside an observable model must not participate in observation, use `@ObservationIgnored` intentionally.
- If you place wrapper-backed properties inside an observable model, verify compile behavior and update propagation in the target toolchain instead of assuming it works like plain stored state.

```swift
struct SettingsView: View {
    @AppStorage("showHints") private var showHints = true

    var body: some View {
        Toggle("Show Hints", isOn: $showHints)
    }
}
```

## Focus and Forms

- Use `Bool` focus state for one field.
- Use a `Hashable` enum for multiple fields.
- In forms, move focus deliberately on submit instead of scattering keyboard logic.

```swift
enum Field: Hashable {
    case email
    case password
}

@FocusState private var focusedField: Field?
```

## Bindings and Effects

- Avoid `Binding(get:set:)` in `body` when direct bindings plus `onChange` or explicit actions are simpler.
- Prefer numeric `TextField` bindings with format styles over string parsing for numeric entry.

```swift
@State private var score = 0

TextField("Score", value: $score, format: .number)
    .keyboardType(.numberPad)
```

## Legacy Interop

- Keep `ObservableObject`, `@Published`, `@StateObject`, `@ObservedObject`, and `@EnvironmentObject` when a migration would be invasive or when a library already exposes them.
- When touching legacy code, preserve the existing ownership semantics even if you do not fully migrate to Observation in the same change.
