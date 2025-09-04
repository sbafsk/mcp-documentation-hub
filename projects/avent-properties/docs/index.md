# Avent Properties - Project Overview

> **AI Context**: This is the single source of truth for project overview.
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`

## Project Overview

Avent Properties is a **luxury real estate platform** targeting High Net Worth Individuals (HNWIs) from Dubai/UAE for premium properties in Uruguay's most exclusive coastal destinations.

## Quick Start

1. **Setup**: See `docs/guides/setup.md` for environment setup
2. **Status**: Check `docs/status/progress.yaml` for current progress
3. **Architecture**: Review `docs/architecture/overview.md` for technical details

## Tech Stack

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS
- **Backend**: Supabase (MVP) → AWS (Production)
- **API**: Hybrid GraphQL (Supabase + Apollo)
- **Database**: PostgreSQL via Supabase
- **Authentication**: Supabase Auth with JWT

## Project Structure

```
Avent Properties/
├── .ai/                    # AI-specific configuration
├── docs/                   # Project documentation
│   ├── status/            # Current state only
│   ├── architecture/      # Technical implementation
│   └── guides/            # How-to documentation
├── standards/              # Immutable standards
└── [src/]                 # Application source code
```

## Current Status

- **Phase**: Foundation Development & MCP Integration
- **MVP Features**: 100% Complete (8/8) ✅
- **GraphQL Migration**: 100% Complete ✅
- **Documentation**: ✅ **CONSOLIDATED** - Single source of truth established
- **Next Priority**: MCP Integration & Enhanced AI Development Experience

## Key Features

- **Property Listings**: Browse and filter luxury properties with advanced search
- **Tour Reservations**: Book premium tours with 10% deposit system
- **Multi-Role Access**: Client, Agency, and Admin dashboards
- **GraphQL API**: Hybrid architecture with Supabase backend
- **Modern UI**: Glassmorphism design with gold accents
- **Responsive Design**: Mobile-first approach

## Related Documentation

- [Current Status](docs/status/progress.yaml)
- [Architecture](docs/architecture/overview.md)
- [Setup Guide](docs/guides/setup.md)
- [Coding Standards](standards/coding.md)

## Getting Help

- Start with this file for project overview
- Check status files for current progress
- Refer to specific guides for implementation details
- Follow coding standards for development