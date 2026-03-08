# Preview Strategies

Read this file for `#Preview`, sample data, or keeping previews representative without leaking into runtime code.

- `[Prefer]` Preview the smallest useful component and at least one feature-level composition.
- `[Prefer]` Cover loading, empty, error, long-text, and large Dynamic Type where they materially affect layout.
- `[Prefer]` Inject sample data and stubs; previews should not depend on live network, disk, or app launch wiring.
- `[Consider]` Keep preview-only helpers near the feature when they are small; extract shared fixtures only when reuse is real.
- `[Only when]` Keep `PreviewProvider` for older toolchains or repos that still standardize on it.
