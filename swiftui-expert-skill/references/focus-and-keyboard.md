# Focus and Keyboard

Read this file for `@FocusState`, submit flow, keyboard dismissal, or validation-driven focus.

- `[Prefer]` Use `@FocusState Bool` for one field and `@FocusState Field?` for multiple fields.
- `[Prefer]` Advance focus in `onSubmit` rather than scattering tap handlers or imperative responder code.
- `[Prefer]` Restore focus to the first invalid field after validation fails.
- `[Prefer]` Keep keyboard behavior local to the form or editor that owns it.
- `[Consider]` Dismiss focus when committing a finished action, not on every transient state change.
- `[Only when]` Reach for UIKit or AppKit responder bridging only when the SwiftUI focus APIs cannot express the interaction.
