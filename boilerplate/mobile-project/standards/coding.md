# Coding Standards - [Project Name] (Mobile)

> **AI Context**: This is the single source of truth for code conventions.
> For current status: see `../docs/status/progress.yaml`
> For project overview: see `../docs/index.md`

## Language & Lints

- **Null-safety**: Required
- **Lints**: `flutter_lints` or `very_good_analysis`
- **Formatting**: `dart format`

## Project Structure

```
lib/
├── app/            # app initialization, themes, routes
├── features/       # feature-first organization
│   └── feature_x/
│       ├── data/   # datasources, repositories, models
│       ├── domain/ # entities, use cases
│       └── presentation/ # widgets, blocs/providers
└── shared/         # shared utils, widgets
```

## State Management

- Prefer [Riverpod | BLoC] for testability
- Avoid global mutable state; use providers/cubits/blocs

## Networking

- Use Dio/http with interceptors
- Centralize error mapping and retries

## Models & Serialization

- Use `freezed` + `json_serializable`
- Immutability and copyWith support

## Testing

- Unit tests for use cases and repositories
- Widget tests for UI components
- Integration tests for end-to-end flows

## Performance

- Use `const` widgets where possible
- Memoize selectors; avoid unnecessary rebuilds
- Lazy-load heavy resources

## Security

- Store secrets in secure storage
- Validate all inputs; sanitize external data
- HTTPS only; consider certificate pinning
