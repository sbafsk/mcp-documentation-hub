# System Architecture Overview

> **AI Context**: This is the single source of truth for system architecture.
> For current status: see `../status/progress.yaml`
> For implementation details: see specific architecture files

## Architecture Overview

Sebastián's Personal Site follows a modern static landing page architecture with dynamic contact form functionality using Next.js 15 and shadcn/ui components.

## High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   API Layer     │    │   External      │
│   (Next.js)     │◄──►│   (Next.js)     │◄──►│   (Email)       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   UI Components │    │   Form Handler  │    │   Email Service │
│   (shadcn/ui)   │    │   (API Route)   │    │   (Nodemailer)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Technology Stack

### Frontend
- **Framework**: Next.js 15 with App Router
- **Language**: TypeScript
- **Styling**: TailwindCSS
- **State Management**: React Context + Hooks
- **UI Components**: shadcn/ui + Radix UI primitives

### Backend
- **Platform**: Next.js API Routes
- **API**: Next.js built-in API routes
- **Database**: None (static site)
- **Authentication**: None (public site)

### Infrastructure
- **Deployment**: Vercel (recommended)
- **Monitoring**: Vercel Analytics (optional)
- **CI/CD**: GitHub Actions (optional)

## Key Architectural Decisions

1. **Static Site Generation**: Use Next.js static generation for optimal performance
2. **API Routes for Contact**: Single API route for contact form submission
3. **Component-Based Architecture**: Modular components using shadcn/ui patterns
4. **Responsive Design**: Mobile-first approach with TailwindCSS
5. **Accessibility First**: Built-in accessibility with Radix UI primitives

## Site Structure

### Page Components
```
app/
├── page.tsx              # Home page (all sections)
├── layout.tsx            # Root layout with navigation
├── globals.css           # Global styles and TailwindCSS
└── api/
    └── contact/
        └── route.ts      # Contact form API endpoint
```

### Section Components
```
components/
├── sections/
│   ├── HeroSection.tsx
│   ├── AboutSection.tsx
│   ├── ExperienceSection.tsx
│   ├── SkillsSection.tsx
│   ├── ProjectsSection.tsx
│   ├── ContactSection.tsx
│   └── CVDownloadSection.tsx
├── ui/                   # shadcn/ui components
├── forms/
│   └── ContactForm.tsx   # Contact form with validation
└── layout/
    ├── Header.tsx        # Navigation header
    └── Footer.tsx        # Site footer
```

## Data Flow

### Static Content
- **Profile Data**: Hardcoded in components or data files
- **Project Data**: Structured data files for easy updates
- **Skills Data**: Organized skill categories with proficiency levels

### Dynamic Content
- **Contact Form**: Form submission → API route → Email service
- **Form Validation**: Client-side with Zod, server-side validation
- **Error Handling**: User-friendly error messages and success feedback

## Performance Considerations

### Optimization Strategies
- **Static Generation**: Pre-render all static content
- **Image Optimization**: Use Next.js Image component
- **Code Splitting**: Lazy load non-critical components
- **Bundle Analysis**: Monitor bundle size with Next.js analytics

### Core Web Vitals Targets
- **LCP**: < 2.5s
- **FID**: < 100ms
- **CLS**: < 0.1

## Security Considerations

### Form Security
- **Input Validation**: Client and server-side validation
- **Rate Limiting**: Prevent spam submissions
- **CSRF Protection**: Built-in Next.js protection
- **Email Sanitization**: Clean user inputs before sending

### Deployment Security
- **HTTPS Only**: Enforce secure connections
- **Environment Variables**: Secure API keys and secrets
- **Content Security Policy**: Prevent XSS attacks

## Related Documentation

- [Design System](../design-system/components.md)
- [Contact Form API](api.md)
- [Component Architecture](components.md)
- [Current Status](../status/progress.yaml)

