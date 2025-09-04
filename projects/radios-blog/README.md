# 📻 Radios Blog - Vintage Radio Collection & Blog Platform

**A platform showcasing vintage radio collections for sale, featuring blog posts about radio station history, and displaying a gallery of vintage postcards.**

## 🎯 Project Overview

Radios Blog is a modern web platform designed for vintage radio enthusiasts, collectors, and history buffs. The platform combines e-commerce functionality for vintage radio sales with rich content about radio history and a curated collection of vintage postcards.

### Key Features
- **Vintage Radio Listings**: Browse and purchase vintage radios with detailed specifications
- **Blog System**: Articles about radio history, station information, and technology
- **Postcard Gallery**: Vintage postcard collection with categorization and history
- **User Collections**: Create and share personal collections of radios and postcards
- **Admin Panel**: Content management for blog posts and collections

---

## 📚 **Documentation & Getting Started**

**🎯 For complete project documentation, start with `docs/index.md`**

This root README provides quick setup instructions. For comprehensive documentation including:
- Current project status and progress
- Development standards and guidelines
- Business requirements and roadmap
- MCP integration setup
- Technical implementation details

**→ Go to `docs/index.md` for the complete picture**

## 🛠️ Tech Stack

- **Frontend**: Next.js 15 (App Router) + TypeScript + React 19
- **Styling**: TailwindCSS + shadcn/ui + Radix UI
- **Backend**: Supabase (PostgreSQL + Auth + Storage)
- **API**: REST API with potential GraphQL migration
- **State Management**: React Context + Hooks
- **Forms**: React Hook Form + Zod validation
- **Testing**: Jest + React Testing Library + Playwright
- **Package Manager**: Yarn
- **Deployment**: Vercel (MVP)

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ 
- Yarn package manager
- Supabase account and project
- v0.app access (UI design provided by another agent)

### 1. Clone and Install
```bash
git clone <repository-url>
cd radios-blog
yarn install
```

### 2. Environment Setup
```bash
cp .env.example .env.local
```

Fill in your environment variables:
```env
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# App Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=Radios Blog
```

### 3. Database Setup
1. Create a new Supabase project
2. Run the database migrations (see `docs/guides/setup.md`)
3. Set up Row Level Security (RLS) policies
4. Configure authentication settings

### 4. Development
```bash
# Start development server
yarn dev

# Run tests
yarn test

# Run E2E tests
yarn test:e2e

# Type checking
yarn type-check

# Linting and formatting
yarn lint
yarn format
```

## 📁 Project Structure

```
radios-blog/
├── .ai/                        # AI-specific configuration
├── app/                        # Next.js App Router pages
│   ├── api/                   # API routes
│   ├── auth/                  # Authentication pages
│   ├── radios/                # Radio listings
│   ├── blog/                  # Blog posts
│   ├── postcards/             # Postcard gallery
│   ├── collections/           # User collections
│   └── admin/                 # Admin panel
├── components/                 # React components
│   ├── ui/                    # shadcn/ui components
│   ├── radios/                # Radio-specific components
│   ├── blog/                  # Blog components
│   └── postcards/             # Postcard components
├── lib/                       # Utilities and configurations
│   ├── supabase.ts            # Supabase client
│   ├── api/                   # API client functions
│   └── utils/                 # Utility functions
├── docs/                      # Project documentation
│   ├── index.md               # Single entry point
│   ├── status/                # Current state only
│   ├── architecture/          # Technical implementation
│   └── guides/                # How-to documentation
├── standards/                 # Immutable standards
├── __tests__/                 # Test files
└── public/                    # Static assets
```

## 🔐 Authentication & Authorization

The platform supports multiple user roles:

- **Guest**: Browse radios, read blog posts, view postcards
- **User**: Create collections, save favorites, make inquiries
- **Seller**: List radios for sale, manage listings
- **Admin**: Full access, content management, user management

Authentication is handled via Supabase Auth with JWT tokens and Row Level Security (RLS) policies.

## 🗄️ Database Schema

### Core Tables
- `users` - User accounts with role-based access
- `vintage_radios` - Radio listings with specifications and images
- `blog_posts` - Blog content about radio history
- `vintage_postcards` - Postcard collection with categorization
- `collections` - User-created collections
- `transactions` - Sales and commission tracking

## 🎨 Design System

### UI Design
- **Design Source**: v0.app (provided by another agent)
- **Implementation**: TailwindCSS + shadcn/ui components
- **Theme**: Responsive design with accessibility focus
- **Components**: Reusable component library

### Brand Identity
- **Colors**: Vintage-inspired color palette
- **Typography**: Readable fonts for content-heavy pages
- **Imagery**: High-quality vintage radio and postcard images
- **Layout**: Clean, organized interface for easy navigation

## 🧪 Testing Strategy

- **Unit Tests**: Jest + React Testing Library for components
- **Integration Tests**: API endpoints and database operations
- **E2E Tests**: Playwright for critical user flows
- **Coverage**: Minimum 80% coverage requirement

## 📦 Available Scripts

```bash
# Development
yarn dev              # Start development server
yarn build            # Build for production
yarn start            # Start production server

# Testing
yarn test             # Run unit tests
yarn test:ci          # Run tests with coverage
yarn test:e2e         # Run E2E tests

# Code Quality
yarn lint             # Run ESLint
yarn format           # Format with Prettier
yarn type-check       # TypeScript type checking

# Database
yarn db:migrate       # Run database migrations
yarn db:seed          # Seed database with sample data
yarn db:studio        # Open Prisma Studio

# Git Hooks (automatic)
# pre-commit: lint, format, type-check
# pre-push: full test suite
```

## 🚀 Deployment

### MVP Deployment (Vercel)
1. Connect GitHub repository to Vercel
2. Set environment variables in Vercel dashboard
3. Deploy automatically on push to main branch

### Future AWS Migration
- Database: Amazon RDS (PostgreSQL)
- Storage: S3 + CloudFront for images
- API: AWS Lambda + API Gateway
- Frontend: Amplify or CloudFront

## 📚 Documentation Structure

**🎯 Complete documentation is organized in the `/docs` folder**

### **Quick Navigation**
- **`docs/index.md`** - **COMPLETE PROJECT OVERVIEW** (Start here!)
- **`docs/status/progress.yaml`** - Current project status and progress
- **`docs/architecture/overview.md`** - System architecture overview
- **`docs/guides/setup.md`** - Environment setup guide

### **For AI Assistants & Developers**
- **`standards/`** - Development standards and patterns
- **`docs/`** - Project status and implementation guides
- **`.ai/`** - AI-specific configuration and context

**→ See `docs/index.md` for complete documentation structure and navigation**

## 🤖 AI Development Assistance

### **MCP Integration**
We use Model Context Protocol (MCP) to enhance AI development assistance with 2-server setup (docs + standards).

**→ See `docs/guides/setup.md` for complete MCP setup and testing instructions**

## 🤝 Contributing

1. **Follow coding standards** in `standards/coding.md`
2. **Write tests** for new features
3. **Use conventional commit messages**
4. **Create feature branches** from `dev`
5. **Submit pull requests** for review
6. **Update documentation** when making changes

**→ See `docs/guides/setup.md` for documentation update workflow**

## 📄 License

Private project - All rights reserved

---

**Built with ❤️ by the Radios Blog Team**

---

## 🔄 Documentation Status

- **Last Updated**: January 2025
- **Documentation**: ✅ **AI-FRIENDLY ARCHITECTURE** - Single source of truth with machine-readable data
- **MCP Integration**: ✅ Ready for implementation
- **UI Design**: 🔄 Pending from v0.app agent
- **Next Review**: Monthly or when major changes occur

**📚 For complete documentation, start with `docs/index.md`**

---

## 🎯 **Quick Start**

1. **Read this file** for project overview and setup
2. **Go to `docs/index.md`** for complete documentation
3. **Check `docs/status/progress.yaml`** for current status
4. **Follow `docs/guides/setup.md`** for keeping docs updated