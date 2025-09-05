# [Project Name] (Mobile) - Project Overview

> **AI Context**: This is the single source of truth for project overview.
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`

## Project Overview

[Project Name] is a mobile application built with Flutter and Dart, following AI-friendly documentation architecture principles.

### Mission Statement
[Brief mission statement or project purpose]

### Target Audience
[Who this project is built for]

## Quick Start

1. **Setup**: See `docs/guides/setup.md` for environment setup
2. **Status**: Check `docs/status/progress.yaml` for current progress
3. **Architecture**: Review `docs/architecture/overview.md` for technical details
4. **Standards**: Follow `standards/coding.md` for development guidelines

## Tech Stack

- **Mobile**: Flutter (Dart)
- **Backend**: [Your Backend Stack]
- **API**: [Your API Technology]
- **Local Storage**: [Hive | SharedPreferences | SQLite]
- **Authentication**: [Your Auth Solution]
- **CI/CD**: [Codemagic | GitHub Actions | Bitrise]
- **Testing**: Unit, Widget, Integration tests
- **Monitoring**: [Crashlytics | Sentry]

## Project Structure

```
[Project Name]/
├── .ai/                    # AI-specific configuration
│   ├── context.yaml       # Master context file
│   ├── mcp-config.json    # MCP server configuration
│   └── agent-instructions.md # AI behavior guidelines
├── docs/                   # Project documentation
│   ├── index.md           # This file - single entry point
│   ├── status/            # Current state only
│   │   ├── progress.yaml  # Machine-readable status
│   │   └── priorities.md  # Current priorities only
│   ├── architecture/      # Technical implementation
│   │   ├── overview.md    # System architecture
│   │   ├── api.md         # API integration patterns
│   │   └── local_storage.md # Local storage strategies
│   └── guides/            # How-to documentation
│       ├── setup.md       # Environment setup (Flutter/Dart)
│       └── deployment.md  # Deployment (Android/iOS)
├── standards/              # Immutable standards
│   ├── coding.md          # Code conventions (Dart/Flutter)
│   └── patterns.md        # Architectural patterns (BLoC/MVVM)
└── [src/]                 # Application source code
```

## Current Status

- **Phase**: [Current Development Phase]
- **Completion**: [Overall completion percentage]%
- **Last Updated**: [Last update date]
- **Next Priority**: [Next major milestone or feature]

### Recent Achievements
- ✅ [Recent achievement 1]
- ✅ [Recent achievement 2]
- ✅ [Recent achievement 3]

### Current Focus
- 🔄 [Current focus area 1]
- 🔄 [Current focus area 2]
- 🔄 [Current focus area 3]

## Architecture Highlights

### Design Principles
- **Single Source of Truth**: Each piece of information exists in exactly one location
- **AI-First Design**: Structure optimized for AI agent consumption
- **Progressive Disclosure**: Information layers from high-level to implementation details
- **Machine-Readable Data**: Status and progress in structured formats

### Development Workflow

1. **Clone Repository**
2. **Install Flutter SDK**
3. **Run `flutter doctor`**
4. **Install Dependencies**: `flutter pub get`
5. **Start Development**: `flutter run`

## Related Documentation

- [Current Status](status/progress.yaml)
- [System Architecture](architecture/overview.md)
- [Setup Guide](guides/setup.md)
- [Coding Standards](../standards/coding.md)
