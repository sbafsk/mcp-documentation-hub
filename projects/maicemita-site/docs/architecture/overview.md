# System Architecture Overview - Maicemita Site

> **AI Context**: This is the single source of truth for system architecture.
> For current status: see `../status/progress.yaml`
> For implementation details: see specific architecture files

## Architecture Overview

Maicemita Site follows a modern static site generation (SSG) architecture optimized for fast loading, mobile performance, and simple maintenance. The architecture prioritizes user experience and business growth.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │     API Layer   │    │    Database     │
│   (Next.js)     │◄──►│   (API Routes)  │◄──►│   (Vercel PG)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   UI Components │    │   Form Handling │    │   Order Storage │
│   (shadcn/ui)   │    │   (Validation)  │    │   (PostgreSQL)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript
- **Styling**: TailwindCSS + shadcn/ui + Radix UI
- **State Management**: React useState/useReducer (simple state)
- **Design System**: shadcn/ui components

### Backend
- **Platform**: Next.js API Routes
- **Database**: Vercel Postgres (MVP)
- **Authentication**: None for MVP (future: NextAuth.js)
- **File Storage**: Vercel Blob for images

### Infrastructure
- **Deployment**: Vercel with automatic deployments
- **CDN**: Vercel Edge Network (global distribution)
- **Monitoring**: Vercel Analytics + Google Analytics
- **Domain**: Custom domain with SSL

## Key Architectural Decisions

### 1. Static Site Generation (SSG)
- **Approach**: Pre-rendered pages for maximum performance
- **Benefits**: Fast loading, excellent SEO, reduced server costs
- **Implementation**: Next.js static generation with ISR for dynamic content

### 2. Component Architecture
- **Pattern**: Atomic design with shadcn/ui components
- **State Management**: Local state with React hooks
- **Performance**: Component-level optimization with React.memo

### 3. API Strategy
- **Pattern**: Next.js API routes for form handling
- **Database**: Vercel Postgres for order storage
- **Validation**: Client and server-side validation

### 4. Mobile-First Design
- **Approach**: Mobile-first responsive design
- **Performance**: Optimized images and minimal JavaScript
- **UX**: Touch-friendly interfaces and fast interactions

## Data Flow Architecture

### 1. Product Showcase Flow
```
Static Pages → Image Optimization → CDN Delivery → User Browser
```

### 2. Order Processing Flow
```
Contact Form → Validation → API Route → Database → Email Notification
```

### 3. Content Management Flow
```
Content Updates → Git Push → Vercel Build → Automatic Deployment
```

## Performance Considerations

### 1. Frontend Optimization
- **Code Splitting**: Automatic with Next.js App Router
- **Image Optimization**: Next.js Image component with WebP
- **Bundle Optimization**: Tree shaking and minimal dependencies

### 2. Backend Optimization
- **API Routes**: Serverless functions with cold start optimization
- **Database**: Connection pooling with Vercel Postgres
- **Caching**: Static generation with ISR for dynamic content

### 3. Infrastructure Optimization
- **CDN**: Global edge distribution via Vercel
- **SSL**: Automatic HTTPS with Let's Encrypt
- **Compression**: Gzip/Brotli compression

## Scalability Strategy

### 1. Horizontal Scaling
- **Frontend**: CDN distribution handles traffic spikes
- **API**: Serverless functions auto-scale
- **Database**: Vercel Postgres scales automatically

### 2. Vertical Scaling
- **Database**: Upgrade to higher Vercel Postgres tier
- **Functions**: Increase memory/CPU limits
- **Storage**: Scale Vercel Blob storage

### 3. Future Migration Path
- **Phase 1**: MVP with Vercel Postgres
- **Phase 2**: Migrate to Supabase for advanced features
- **Phase 3**: Add payment processing and user accounts

## Development Patterns

### 1. Component Patterns
- **Atomic Design**: Atoms, molecules, organisms, templates
- **Custom Hooks**: Reusable logic for forms and data fetching
- **Error Boundaries**: Graceful error handling

### 2. API Patterns
- **RESTful Routes**: Standard HTTP methods and status codes
- **Error Handling**: Consistent error responses
- **Validation**: Zod schema validation

### 3. Testing Patterns
- **Unit Testing**: Component testing with React Testing Library
- **Integration Testing**: API route testing
- **E2E Testing**: Critical user flows

## Security Architecture

### 1. Data Protection
- **Input Validation**: Client and server-side validation
- **SQL Injection**: Parameterized queries with Vercel Postgres
- **XSS Prevention**: React's built-in XSS protection

### 2. Infrastructure Security
- **HTTPS**: Automatic SSL with Vercel
- **Environment Variables**: Secure configuration management
- **Rate Limiting**: API route rate limiting

### 3. Privacy Compliance
- **GDPR**: Basic compliance for contact forms
- **Data Minimization**: Only collect necessary information
- **Consent**: Clear privacy policy and consent forms

## Deployment Architecture

### 1. Environment Strategy
- **Development**: Local Next.js dev server
- **Staging**: Vercel preview deployments
- **Production**: Vercel production with custom domain

### 2. CI/CD Pipeline
- **Build Process**: Automatic builds on git push
- **Testing**: Automated tests in CI pipeline
- **Deployment**: Zero-downtime deployments

### 3. Rollback Strategy
- **Automatic Rollback**: Vercel automatic rollback on failure
- **Manual Rollback**: Git-based rollback to previous commits
- **Database Rollback**: Point-in-time recovery with Vercel Postgres

## Monitoring & Observability

### 1. Application Monitoring
- **Performance**: Vercel Analytics and Core Web Vitals
- **Error Tracking**: Vercel built-in error monitoring
- **User Analytics**: Google Analytics integration

### 2. Infrastructure Monitoring
- **Uptime**: Vercel status page and monitoring
- **Performance**: Edge function performance metrics
- **Database**: Vercel Postgres performance monitoring

### 3. Business Metrics
- **Conversion Tracking**: Google Analytics goals
- **User Behavior**: Heatmaps and session recordings
- **Order Analytics**: Custom analytics for business insights

## Future Architecture Considerations

### 1. Payment Integration
- **Stripe**: Secure payment processing
- **Webhooks**: Real-time payment status updates
- **PCI Compliance**: Stripe handles compliance

### 2. User Management
- **NextAuth.js**: Authentication and session management
- **User Profiles**: Customer account management
- **Order History**: Customer order tracking

### 3. Advanced Features
- **Inventory Management**: Real-time stock tracking
- **Multi-language**: Internationalization support
- **PWA**: Progressive Web App capabilities

## Related Documentation

- [API Specifications](api.md)
- [Database Schema](database.md)
- [Component Architecture](components.md)
- [Security Model](security.md)
- [Deployment Guide](../guides/deployment.md)
