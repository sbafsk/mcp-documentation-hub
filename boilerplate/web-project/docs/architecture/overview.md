# System Architecture Overview

> **AI Context**: This is the single source of truth for system architecture.
> For current status: see `../status/progress.yaml`
> For implementation details: see specific architecture files

## Architecture Overview

[Project Name] follows a modern full-stack web application architecture with clear separation of concerns.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │     API Layer   │    │    Backend      │
│   (Next.js)     │◄──►│   (GraphQL)     │◄──►│   (Supabase)    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   UI Components │    │   Data Layer    │    │   Database      │
│   (React)       │    │   (Apollo)      │    │   (PostgreSQL)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript
- **Styling**: TailwindCSS
- **State Management**: React Context + Hooks
- **UI Components**: Custom design system

### Backend
- **Platform**: [Your Backend Choice]
- **API**: [Your API Choice]
- **Database**: [Your Database Choice]
- **Authentication**: [Your Auth Choice]

### Infrastructure
- **Deployment**: [Your Deployment Choice]
- **Monitoring**: [Your Monitoring Choice]
- **CI/CD**: [Your CI/CD Choice]

## Key Architectural Decisions

1. **Hybrid API Approach**: [Explain your API strategy]
2. **Component Architecture**: [Explain your component strategy]
3. **State Management**: [Explain your state management approach]
4. **Security Model**: [Explain your security approach]

## Related Documentation

- [Database Schema](database.md)
- [API Specifications](api.md)
- [Component Architecture](components.md)
- [Security Model](security.md)