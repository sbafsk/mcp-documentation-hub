# Environment Setup Guide

> **AI Context**: This is the single source of truth for environment setup.
> For current status: see `../status/progress.yaml`
> For project overview: see `../index.md`

## Prerequisites

Before setting up [Project Name], ensure you have the following installed:

- **Node.js**: Version 18+ (recommended: LTS)
- **npm** or **yarn**: Package manager
- **Git**: Version control
- **[Other Requirements]**: [List any other prerequisites]

## Quick Setup (5 minutes)

### 1. Clone the Repository
```bash
git clone [your-repository-url]
cd [project-name]
```

### 2. Install Dependencies
```bash
npm install
# or
yarn install
```

### 3. Environment Configuration
```bash
cp .env.example .env.local
# Edit .env.local with your configuration
```

### 4. Start Development Server
```bash
npm run dev
# or
yarn dev
```

Your application should now be running at `http://localhost:3000`

## Detailed Setup

### Environment Variables

Create a `.env.local` file with the following variables:

```env
# Database
DATABASE_URL="[your-database-url]"
DATABASE_AUTH_TOKEN="[your-auth-token]"

# Authentication
NEXTAUTH_SECRET="[your-secret]"
NEXTAUTH_URL="http://localhost:3000"

# External Services
[EXTERNAL_SERVICE_KEY]="[your-api-key]"
[EXTERNAL_SERVICE_SECRET]="[your-secret]"

# Feature Flags
ENABLE_[FEATURE]="true"
```

### Database Setup

1. **Create Database**: [Instructions for your database]
2. **Run Migrations**: `npm run db:migrate`
3. **Seed Data**: `npm run db:seed` (optional)

### Authentication Setup

1. **Configure Provider**: [Your auth provider setup]
2. **Set Environment Variables**: [Required auth variables]
3. **Test Authentication**: [How to verify auth is working]

## Development Workflow

### Available Scripts

```json
{
  "scripts": {
    "dev": "Next.js development server",
    "build": "Production build",
    "start": "Production server",
    "lint": "Code linting",
    "test": "Run tests",
    "db:migrate": "Database migrations",
    "db:seed": "Seed database with sample data"
  }
}
```

### Code Quality Tools

- **ESLint**: Code linting and formatting
- **Prettier**: Code formatting
- **TypeScript**: Type checking
- **Husky**: Git hooks for quality checks

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Kill process using port 3000
lsof -ti:3000 | xargs kill -9
```

#### Database Connection Issues
- Verify environment variables
- Check database service status
- Confirm network connectivity

#### Build Errors
- Clear node_modules: `rm -rf node_modules && npm install`
- Clear Next.js cache: `rm -rf .next`

## Next Steps

After successful setup:

1. **Review Architecture**: See `../architecture/overview.md`
2. **Check Status**: See `../status/progress.yaml`
3. **Start Development**: Begin with [first feature]
4. **Run Tests**: Ensure everything is working

## Related Documentation

- [Project Overview](../index.md)
- [System Architecture](../architecture/overview.md)
- [Current Status](../status/progress.yaml)
- [Coding Standards](../../standards/coding.md)