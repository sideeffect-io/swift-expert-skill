# Sheet Patterns

Read this file for sheet ownership, enum sheets, popovers, or detent-related modal work.

- `[Prefer]` Use `.sheet(item:)` when modal state is naturally a selected model or enum case.
- `[Prefer]` Let the item or boolean binding define whether the modal exists; SwiftUI resets the presenting binding on dismissal.
- `[Prefer]` Let the presented view own dismiss, save, and cancel when the workflow is local to the sheet.
- `[Prefer]` Keep sheet state narrow and typed instead of accumulating one boolean per modal.
- `[Consider]` Use `fullScreenCover` only when the task genuinely needs a full-screen modal experience.
- `[Consider]` Add detents, sizing, or background styling only when the sheet benefits from that extra structure.
- `[Only when]` Use popovers for anchored, lightweight tasks rather than replacing every sheet with one.
