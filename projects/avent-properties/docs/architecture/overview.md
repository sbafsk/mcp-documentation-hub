# System Architecture Overview - Avent Properties

> **AI Context**: This is the single source of truth for system architecture.
> For current status: see `../status/progress.yaml`
> For implementation details: see specific architecture files

## Architecture Overview

Avent Properties follows a modern full-stack web application architecture with clear separation of concerns and hybrid GraphQL approach.

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
- **Styling**: TailwindCSS + shadcn/ui + Radix UI
- **State Management**: Redux Toolkit + React Query
- **Design System**: Glassmorphism with gold accents

### Backend
- **Platform**: Supabase (MVP) → AWS (Production)
- **API**: Hybrid GraphQL (Supabase auto-generated + Custom Apollo Server)
- **Database**: PostgreSQL via Supabase
- **Authentication**: Supabase Auth with JWT tokens

### Infrastructure
- **Deployment**: Vercel (MVP) → AWS (Production)
- **Monitoring**: Vercel Analytics + Sentry
- **CI/CD**: GitHub Actions
- **Storage**: Supabase Storage → S3 (Production)

## Key Architectural Decisions

### 1. Hybrid GraphQL Approach
- **Supabase GraphQL**: 72% of endpoints (simple CRUD operations)
- **Custom Apollo Server**: 28% of endpoints (complex business logic)
- **Benefits**: Automatic schema generation + custom business rules

### 2. Component Architecture
- **Pattern**: Container/Presenter separation
- **State Management**: Local state + Redux for global state
- **Performance**: React.memo, useCallback, useMemo optimization

### 3. State Management Strategy
- **Local State**: useState for component-specific data
- **Global State**: Redux Toolkit for application-wide state
- **Server State**: React Query for API data and caching

### 4. Security Model
- **Authentication**: Supabase Auth with JWT
- **Authorization**: Role-based access control (ADMIN, CLIENT, AGENCY)
- **Data Protection**: Row Level Security (RLS) policies
- **API Security**: Rate limiting, input validation, CORS

## Data Flow Architecture

### 1. User Authentication Flow
```
User Login → Supabase Auth → JWT Token → Protected Routes → RLS Policies
```

### 2. Property Data Flow
```
Property Request → GraphQL Query → Supabase/PostgreSQL → React Query Cache → UI
```

### 3. Reservation Flow
```
Tour Request → Apollo Server → Business Logic → Database → Email Notification
```

## Performance Considerations

### 1. Frontend Optimization
- **Code Splitting**: Dynamic imports for route-based splitting
- **Image Optimization**: Next.js Image component with WebP support
- **Bundle Optimization**: Tree shaking and dead code elimination

### 2. Backend Optimization
- **Database Indexing**: Strategic indexes for common queries
- **Query Optimization**: N+1 query elimination with joins
- **Caching Strategy**: React Query with background refetching

### 3. Infrastructure Optimization
- **CDN**: Vercel Edge Network → CloudFront (AWS)
- **Database**: Connection pooling and read replicas
- **API**: Rate limiting and request optimization

## Scalability Strategy

### 1. Horizontal Scaling
- **Frontend**: Multiple Vercel deployments → AWS regions
- **API**: Load balancer with multiple instances
- **Database**: Read replicas and connection pooling

### 2. Vertical Scaling
- **Database**: Instance size increases
- **API**: Memory and CPU optimization
- **Storage**: Tiered storage strategy

### 3. Future Migration Path
- **Phase 1**: MVP on Vercel + Supabase
- **Phase 2**: Business features and optimization
- **Phase 3**: AWS migration for production scale

## Related Documentation

- [Database Architecture](database.md)
- [API Specifications](api.md)
- [Component Architecture](components.md)
- [Security Model](security.md)