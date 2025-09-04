# Maicemita Site - Project Overview

> **AI Context**: This is the single source of truth for project overview.
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`

## Project Overview

Maicemita Site is a simple, beautiful e-commerce website to promote and sell homemade alfajores de maicena (sweet pastry). The site showcases different box sizes and flavors, providing an easy way for customers to discover and order these delicious homemade desserts.

### Mission Statement
Create a fast, mobile-friendly website that beautifully showcases homemade alfajores and makes it easy for customers to place orders, helping grow the Maicemita business.

### Target Audience
Local customers interested in homemade desserts, sweet pastries, and authentic flavors. Focus on mobile users and social media-driven traffic.

## Quick Start

1. **Setup**: See `docs/guides/setup.md` for environment setup
2. **Status**: Check `docs/status/progress.yaml` for current progress
3. **Architecture**: Review `docs/architecture/overview.md` for technical details
4. **Standards**: Follow `standards/coding.md` for development guidelines

## Tech Stack

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS + shadcn/ui
- **Backend**: Next.js API Routes
- **Database**: Vercel Postgres (MVP) / Supabase (future)
- **Deployment**: Vercel with automatic deployments
- **Testing**: Jest + React Testing Library
- **Monitoring**: Vercel Analytics

## Project Structure

```
maicemita-site/
├── .ai/                    # AI-specific configuration
│   ├── context.yaml       # Master context file
│   └── agent-instructions.md # AI behavior guidelines
├── docs/                   # Project documentation
│   ├── index.md           # This file - single entry point
│   ├── status/            # Current state only
│   │   ├── progress.yaml  # Machine-readable status
│   │   └── priorities.md  # Current priorities only
│   ├── architecture/      # Technical implementation
│   │   ├── overview.md    # System architecture
│   │   └── api.md         # API specifications
│   └── guides/            # How-to documentation
│       ├── setup.md       # Environment setup
│       └── deployment.md  # Deployment processes
├── standards/              # Immutable standards
│   ├── coding.md          # Code conventions
│   └── design.md          # Design system guidelines
└── .cursor/               # Cursor AI configuration
    └── mcp.json           # MCP server configuration
```

## Current Status

- **Phase**: Planning & Documentation
- **Completion**: 5%
- **Last Updated**: 2025-01-27
- **Next Priority**: Complete documentation structure and prepare for MVP development

### Recent Achievements
- ✅ Project documentation structure created
- ✅ AI context and MCP integration configured
- ✅ Tech stack and architecture defined

### Current Focus
- 🔄 Setting up development environment
- 🔄 Creating initial project structure
- 🔄 Preparing for UI design implementation

## Key Features

### Core Features (MVP)
- **Product Showcase**: Beautiful gallery of alfajores with high-quality images
- **Box Size Selection**: Small, medium, and large box options
- **Flavor Selection**: Dulce de leche, membrillo, and batata flavors
- **Simple Contact Form**: Easy ordering process with contact information
- **Mobile-Responsive Design**: Optimized for mobile users
- **Fast Loading**: SEO optimized with static site generation

### Planned Features (Phase 2)
- **Online Payment Integration**: Stripe payment processing
- **Inventory Management**: Track available products
- **Order Tracking System**: Customer order status updates
- **Customer Reviews**: Testimonials and feedback system
- **Social Media Integration**: Share products on social platforms

### Future Features (Phase 3)
- **User Accounts**: Customer registration and order history
- **Subscription Service**: Regular delivery for loyal customers
- **Multi-language Support**: Spanish and English versions
- **Advanced Analytics**: Detailed business insights
- **Delivery Integration**: Third-party delivery service integration

## Architecture Highlights

### Design Principles
- **Mobile-First**: Optimized for mobile users and social media traffic
- **Fast Loading**: Static site generation with optimized images
- **Simple UX**: Easy navigation and ordering process
- **Beautiful Design**: Showcase products with high-quality visuals

### Key Architectural Decisions
1. **Next.js 15**: Modern React framework with excellent performance
2. **Static Site Generation**: Fast loading and SEO optimization
3. **Vercel Deployment**: Simple deployment with automatic updates
4. **shadcn/ui Components**: Beautiful, accessible UI components

### Technology Choices
- **Next.js**: Excellent for e-commerce with built-in optimization
- **TailwindCSS**: Rapid styling with consistent design system
- **Vercel**: Zero-config deployment with global CDN
- **TypeScript**: Type safety and better development experience

## Development Workflow

### Getting Started
1. **Clone Repository**: `git clone <repository-url>`
2. **Install Dependencies**: `yarn install`
3. **Environment Setup**: Follow `docs/guides/setup.md`
4. **Start Development**: `yarn dev`

### Development Standards
- **Code Style**: Follow `standards/coding.md`
- **Testing**: Maintain component test coverage
- **Documentation**: Update docs when making changes
- **AI Assistance**: Use Cursor AI with MCP integration

### Available Scripts
```bash
yarn dev          # Start development server
yarn build        # Build for production
yarn test         # Run tests
yarn lint         # Code linting
yarn type-check   # TypeScript checking
```

## AI Development Assistance

### MCP Integration
This project uses Model Context Protocol (MCP) for enhanced AI development assistance:

- **Documentation Server**: Access to all project documentation
- **Standards Server**: Access to coding standards and patterns
- **Context Loading**: Automatic context injection for AI agents

### AI Context Files
- **Master Context**: `.ai/context.yaml` - Project metadata and status
- **Agent Instructions**: `.ai/agent-instructions.md` - AI behavior guidelines
- **MCP Configuration**: `.cursor/mcp.json` - Server setup

### AI-Friendly Features
- **Single Source of Truth**: No duplicate information across files
- **Machine-Readable Status**: YAML-based progress tracking
- **Context Injection**: AI instructions embedded in relevant sections
- **Progressive Disclosure**: Information organized by detail level

## Performance & Quality

### Performance Targets
- **Load Time**: < 2 seconds
- **Response Time**: < 500ms
- **Uptime**: 99.9%
- **Concurrent Users**: 50-100

### Quality Metrics
- **Test Coverage**: 80% component coverage
- **Code Quality**: ESLint + Prettier standards
- **Accessibility**: WCAG 2.1 AA compliance
- **SEO**: Lighthouse score > 90

## Business Context

### Stakeholders
- **Sister (Business Owner)**: Provides products and business requirements
- **Developer (You)**: Technical implementation and maintenance

### Success Metrics
- **User Experience**: Easy product discovery and ordering
- **Mobile Performance**: Fast loading on mobile devices
- **Conversion Rate**: Simple ordering process
- **Business Growth**: Increased orders and customer reach

### Constraints
- **Timeline**: 2-3 weeks for MVP
- **Budget**: Minimal - using free Vercel tier initially
- **Maintenance**: Simple updates and content management
- **Scalability**: Easy to add features as business grows

## Deployment & Infrastructure

### Environments
- **Development**: Local development with Next.js dev server
- **Staging**: Vercel preview deployments for testing
- **Production**: Vercel production deployment with custom domain

### Deployment Strategy
- **Git-Based**: Automatic deployments from main branch
- **Preview Deployments**: Test changes before production
- **Rollback Strategy**: Automatic rollback on deployment failure
- **Monitoring**: Vercel built-in analytics and monitoring

### Infrastructure
- **Hosting**: Vercel with global CDN
- **Database**: Vercel Postgres for MVP
- **Storage**: Vercel Blob for images
- **Analytics**: Vercel Analytics and Google Analytics

## Related Documentation

### Core Documentation
- [Current Status](status/progress.yaml) - Machine-readable project status
- [System Architecture](architecture/overview.md) - Technical architecture details
- [Setup Guide](guides/setup.md) - Environment setup instructions
- [Coding Standards](../standards/coding.md) - Development guidelines

### Additional Resources
- [API Documentation](architecture/api.md) - API specifications
- [Deployment Guide](guides/deployment.md) - Deployment instructions
- [Design System](../standards/design.md) - UI/UX guidelines

## Getting Help

### For Developers
- Start with this file for project overview
- Check status files for current progress
- Refer to specific guides for implementation details
- Follow coding standards for development

### For AI Assistants
- Load `.ai/context.yaml` first for project context
- Check `docs/status/progress.yaml` for current status
- Refer to `standards/coding.md` for code generation
- Use context injection patterns for consistency

### For Stakeholders
- Review this overview for project understanding
- Check status files for progress updates
- Refer to business context for project goals
- Contact development team for specific questions

## Contributing

### Development Process
1. **Fork Repository**: Create your own copy
2. **Create Branch**: Use feature branch naming
3. **Follow Standards**: Adhere to coding standards
4. **Write Tests**: Maintain test coverage
5. **Update Documentation**: Keep docs current
6. **Submit PR**: Request code review

### Documentation Updates
- **Single Source of Truth**: Update information in one place only
- **Context Injection**: Add AI instructions where relevant
- **Machine-Readable**: Use structured formats for status data
- **Progressive Disclosure**: Organize by detail level

---

**This documentation follows AI-friendly architecture principles with single source of truth, machine-readable data, and context injection patterns for optimal AI development assistance.**
