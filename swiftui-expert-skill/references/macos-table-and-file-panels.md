# macOS Table and File Panels

Read this file for macOS-heavy data tables, import/export flows, or file dialog nuance.

- `[Prefer]` Use `Table` for sortable multi-column data; Apple documents sortable `Table(sortOrder:)` initializers on iOS 16+, macOS 12+, and visionOS 1+.
- `[Prefer]` Keep the first column informative because narrow layouts may collapse other columns.
- `[Prefer]` Use `fileImporter` and the related SwiftUI file modifiers before dropping to AppKit panels directly; Apple documents `fileImporter` on iOS 14+, macOS 11+, and visionOS 1+.
- `[Consider]` For imported file URLs, Apple documents that you may need to call `startAccessingSecurityScopedResource()` and later stop access.
- `[Only when]` Use custom AppKit panels only when the SwiftUI modifiers cannot express the required browser options or workflow.
