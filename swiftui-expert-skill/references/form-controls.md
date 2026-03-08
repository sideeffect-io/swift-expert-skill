# Form Controls

Read this file for choosing between `Toggle`, `Picker`, `Stepper`, `Slider`, and `LabeledContent` inside a `Form`.

- `[Prefer]` Use `Form` for grouped data entry, settings, and inspectors.
- `[Prefer]` Use `LabeledContent` when a read-only or read-write value should align with its label in a form row; Apple documents it on iOS 16+, macOS 13+, tvOS 16+, watchOS 9+, and visionOS 1+.
- `[Prefer]` Use `Toggle` for binary state, `Picker` for finite choices, `Stepper` for small incremental changes, and `Slider` for bounded continuous values.
- `[Consider]` Promote a control out of a form only when it needs more space or more direct manipulation.
- `[Only when]` Replace system controls with custom ones only if the product requirement is stronger than the loss of built-in accessibility and platform consistency.
