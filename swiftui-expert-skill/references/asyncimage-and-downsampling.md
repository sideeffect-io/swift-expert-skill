# AsyncImage and Downsampling

Read this file for `AsyncImage`, row thumbnails, or image memory spikes.

- `[Prefer]` Use `AsyncImage` for simple remote-image loading; it is available on iOS 15+, macOS 12+, tvOS 15+, watchOS 8+, and visionOS 1+.
- `[Prefer]` Use the phase-based initializer when loading, failure, and transition states matter.
- `[Prefer]` Keep row images small and explicitly resized or clipped.
- `[Consider]` If you decode raw image data yourself, downsample before display to reduce memory and main-thread cost.
- `[Only when]` Use a custom image pipeline when caching, retry policy, or progressive decoding requirements exceed what `AsyncImage` provides.
