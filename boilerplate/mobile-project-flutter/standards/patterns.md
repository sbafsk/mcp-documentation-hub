# Architectural Patterns (Mobile)

> **AI Context**: This is the single source of truth for architectural patterns.
> For current status: see `../docs/status/progress.yaml`
> For system overview: see `../docs/architecture/overview.md`

## State Management Patterns

### Riverpod
- Providers, StateNotifier, AsyncValue patterns

### BLoC
- Events/States, Cubit for simpler cases

## Repository Pattern

- Abstract data layer with repositories and datasources
- Dependency injection (get_it, riverpod providers)

## Navigation

- GoRouter with declarative routes and redirection
- Deep linking configuration

## Error Handling

- Use Result/Either types
- Global error/reporting hooks

## Dependency Injection

- Prefer riverpod providers or get_it
- Avoid service locators in widgets directly
