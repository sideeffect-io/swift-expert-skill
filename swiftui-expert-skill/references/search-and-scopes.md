# Search and Scopes

Read this file for `searchable`, `searchScopes`, search tabs, or search-specific empty states.

- `[Prefer]` One search source of truth: query, scope, and results should live together.
- `[Prefer]` Add `searchScopes` only when users truly switch search mode, not just sort or filter.
- `[Prefer]` Debounce expensive work with `.task(id:)` or model throttling.
- `[Prefer]` Use `ContentUnavailableView.search` for empty search results when the surrounding UI fits.
- `[Consider]` Use a dedicated search tab only when search is a first-class destination, not just a field on one screen.
- `[Only when]` If the query must survive tab or scene switching, store it above the local view.
