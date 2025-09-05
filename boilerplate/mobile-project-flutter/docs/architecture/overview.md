# System Architecture Overview - [Project Name] (Mobile)

> **AI Context**: This is the single source of truth for system architecture.
> For current status: see `../status/progress.yaml`
> For implementation details: see specific architecture files

## Architecture Overview

[Project Name] follows a modern mobile application architecture with clear separation of concerns and scalable design patterns.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   UI Layer      │    │     Domain      │    │   Data Layer    │
│   (Flutter)     │◄──►│  (Use Cases)    │◄──►│ (API/Storage)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Presentation    │    │  State Mgmt     │    │  Integrations   │
│ Widgets/Pages   │    │ (BLoC/Riverpod) │    │ (Dio/HTTP/Hive) │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Technology Stack

### Mobile
- **Framework**: Flutter (Dart)
- **State Management**: Riverpod (preferred per standards)
- **Navigation**: GoRouter (preferred)
- **Styling**: Material 3 / Cupertino
- **Codegen**: Freezed + JsonSerializable
- **Hooks**: flutter_hooks + hooks_riverpod for composable stateful logic

### Backend & API
- **Platform**: [Your Backend Platform]
- **API**: [REST | GraphQL]
- **Database/Storage**: [Supabase | Firebase | Custom]

### Infrastructure
- **Deployment**: [Play Store | App Store]
- **CI/CD**: [Codemagic | GitHub Actions | Bitrise]
- **Monitoring**: [Crashlytics | Sentry]

## Key Architectural Decisions

### 1. State Management Strategy
- **Local State**: `setState` / ValueNotifiers for simple cases
- **App State**: [Riverpod | BLoC] for composition and testability
- **Async/Data**: Stream-based or Future-based with repositories

### 2. Data & Repository Layer
- **Pattern**: Repository abstraction over API/local storage
- **Caching**: Local cache via Hive/SQLite
- **Error Handling**: Result/Either pattern, typed exceptions

### 3. API & Networking
- **HTTP Client**: Dio or http with interceptors
- **Auth**: Token management, refreshing, secure storage
- **Serialization**: json_serializable / freezed

### 4. Security Model
- **Secure Storage**: Keychain/Keystore for tokens
- **Input Validation**: Form validators, schema when applicable
- **Communication**: HTTPS/TLS, certificate pinning (optional)

## Data Flow

```
User Action → Widget → State (BLoC/Riverpod) → Use Case → Repository → API/Storage → State → UI
```

## Performance Considerations

- **Startup**: Deferred initialization, warmup strategies
- **Rendering**: Avoid rebuilds (selectors, keys, const widgets)
- **Images**: Caching, responsive sizes, placeholders
- **List Views**: Slivers, pagination, virtualization

## Testing Strategy

- **Unit Tests**: Use case, repository, utils
- **Widget Tests**: Widgets with mock dependencies
- **Integration Tests**: Golden tests, end-to-end flows

## Related Documentation

- [API Integration](api.md)
- [Local Storage](local_storage.md)
- [Setup Guide](../guides/setup.md)
- [Deployment Guide](../guides/deployment.md)
