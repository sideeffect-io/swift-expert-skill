# Swift and Hygiene

Read this file when the request includes general Swift quality, concurrency, testing, localization, or project-hygiene concerns around a SwiftUI codebase.

## Modern Swift Defaults

- Prefer `Date.now` over `Date()` when you want the current moment.
- Prefer `URL.documentsDirectory`, `URL.cachesDirectory`, and `appending(path:)` over older `FileManager` path plumbing when the deployment target supports them.
- Prefer `FormatStyle` APIs for user-facing dates, numbers, currencies, and measurements over `String(format:)` or ad hoc formatter strings.
- Prefer text interpolation over concatenating `Text` values with `+`.
- Prefer `if let value` shorthand and expression-style `if` or `switch` when it improves clarity.
- Avoid force unwraps and force `try` unless failure is genuinely unrecoverable and clearly documented.
- Use `localizedStandardContains(_:)` for user-facing search matching instead of simple ASCII-only matching.
- Do not swallow user-triggered errors. Surface them in the UI or return them to the caller intentionally.

```swift
Text(total, format: .currency(code: "EUR"))
Text(Date.now, format: .dateTime.day().month().year())
```

## Concurrency

- Prefer async or await APIs over closure-based or GCD-based equivalents in new code.
- Prefer `.task` and structured concurrency over `DispatchQueue.main.async` or detached fire-and-forget work.
- Protect mutable shared state with actors or `@MainActor` when it is UI-bound.
- Use `Task.sleep(for:)` rather than `Task.sleep(nanoseconds:)`.
- Treat `Task.detached()` as a specialized tool. Reach for it only when you truly need to break actor inheritance and structured cancellation.
- Check `@Sendable` closure boundaries carefully, especially when SwiftUI APIs may invoke work off the main actor.

```swift
.task(id: query) {
    results = await searchService.search(query)
}
```

## SwiftUI and Concurrency Boundaries

- Keep side effects out of `body`.
- If a custom layout, shape, visual effect, or geometry callback may run away from the main actor, capture immutable values instead of reaching into actor-isolated mutable state directly.
- Prefer one explicit async boundary around search, save, sync, or load work rather than mixing timers, GCD, and callback pyramids.

## Security and Persistence Hygiene

- Never commit secrets, tokens, or API keys to the repository.
- Do not store passwords, refresh tokens, or other credentials in `@AppStorage` or `UserDefaults`; use Keychain-backed storage.
- Be deliberate about what is persisted locally, what is cached, and what should clear on sign-out.

## Tests and Maintenance

- Unit tests should cover business rules, parsing, reducers, formatting helpers, and other non-UI logic.
- Use UI tests for end-to-end flows that cannot be validated effectively at a lower layer.
- Add comments only when logic, invariants, or API constraints are not self-evident from the code.
- Preserve existing project tooling such as SwiftLint, SwiftFormat, string catalogs, or generated asset symbols when they are already part of the repo.

## Localization

- Prefer localization-friendly APIs such as interpolation and inflection over manual string building.
- When the project uses `Localizable.xcstrings` or generated string symbols, keep new user-facing strings in that system rather than introducing ad hoc literals in many files.
- Avoid hard-coded date or number formatting strings for user-visible content when a locale-aware format style exists.

## Hygiene Checklist

- Modern Swift APIs where they materially improve clarity
- Structured concurrency instead of GCD
- Actor-safe UI state
- No committed secrets
- Credentials stored outside `UserDefaults`
- Tests for non-trivial logic
- Localization patterns preserved
