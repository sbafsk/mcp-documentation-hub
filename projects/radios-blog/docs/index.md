# Radios Blog - Project Overview

> **AI Context**: This is the single source of truth for project overview.
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`

## Project Overview

Radios Blog is a **vintage radio collection and blog platform** designed to showcase vintage radio collections for sale, feature blog posts about radio station history, and display a gallery of vintage postcards.

## Quick Start

1. **Setup**: See `docs/guides/setup.md` for environment setup
2. **Status**: Check `docs/status/progress.yaml` for current progress
3. **Architecture**: Review `docs/architecture/overview.md` for technical details

## Tech Stack

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS
- **Backend**: Supabase (PostgreSQL + Auth + Storage)
- **API**: REST API with potential GraphQL migration
- **Database**: PostgreSQL via Supabase
- **Authentication**: Supabase Auth with JWT

## Project Structure

```
Radios Blog/
├── .ai/                    # AI-specific configuration
├── docs/                   # Project documentation
│   ├── status/            # Current state only
│   ├── architecture/      # Technical implementation
│   └── guides/            # How-to documentation
├── standards/              # Immutable standards
└── [src/]                 # Application source code
```

## Current Status

- **Phase**: Project Initialization & Planning
- **MVP Features**: 0% Complete - Project just started
- **UI Design**: Will be provided by another agent via v0.app
- **Next Priority**: UI Design & Project Structure Setup

## Key Features

- **Vintage Radio Listings**: Browse and purchase vintage radios with detailed specifications
- **Blog System**: Articles about radio history, station information, and technology
- **Postcard Gallery**: Vintage postcard collection with categorization and history
- **User Collections**: Create and share personal collections of radios and postcards
- **Admin Panel**: Content management for blog posts and collections

## Business Model

- **Radio Sales**: Commission on vintage radio transactions
- **Premium Content**: Subscription access to exclusive blog content
- **Collection Services**: Premium features for serious collectors
- **Advertising**: Targeted ads for radio enthusiasts and collectors

## Target Audience

- **Radio Enthusiasts**: Hobbyists and collectors
- **History Buffs**: People interested in radio broadcasting history
- **Collectors**: Serious vintage radio collectors
- **Researchers**: Students and researchers studying radio history

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