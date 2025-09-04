# Environment Setup Guide - Avent Properties

> **AI Context**: This is the single source of truth for environment setup.
> For current status: see `../status/progress.yaml`
> For project overview: see `../index.md`

## Prerequisites

Before setting up Avent Properties, ensure you have the following installed:

- **Node.js**: Version 18+ (recommended: LTS)
- **Yarn**: Package manager (required)
- **Git**: Version control
- **Supabase account**: For backend services
- **Stripe account**: For payment processing (future)

## Quick Setup (5 minutes)

### 1. Clone the Repository
```bash
git clone [your-repository-url]
cd avent-properties
```

### 2. Install Dependencies
```bash
yarn install
```

### 3. Environment Configuration
```bash
cp .env.example .env.local
# Edit .env.local with your configuration
```

### 4. Start Development Server
```bash
yarn dev
```

Your application should now be running at `http://localhost:3000`

## Detailed Setup

### Environment Variables

Create a `.env.local` file with the following variables:

```env
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# Database URLs
DATABASE_URL=your_supabase_database_url
DIRECT_URL=your_supabase_direct_url

# App Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=Avent Properties

# Security
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3000

# Future: Stripe Configuration
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
```

### Database Setup

1. **Create Supabase Project**: 
   - Go to [supabase.com](https://supabase.com)
   - Create new project
   - Note your project URL and keys

2. **Run Database Migrations**:
   ```bash
   npx prisma migrate dev
   ```

3. **Seed Database** (optional):
   ```bash
   npx prisma db seed
   ```

4. **Verify Connection**:
   ```bash
   npx prisma studio
   ```

### Authentication Setup

1. **Configure Supabase Auth**:
   - Enable email authentication in Supabase dashboard
   - Configure email templates
   - Set up password policies

2. **Test Authentication**:
   - Try user registration
   - Test login/logout
   - Verify role-based access

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
    "test:ci": "Run tests with coverage",
    "test:e2e": "Run E2E tests",
    "type-check": "TypeScript type checking",
    "db:migrate": "Database migrations",
    "db:seed": "Seed database with sample data",
    "db:studio": "Open Prisma Studio"
  }
}
```

### Code Quality Tools

- **ESLint**: Code linting and formatting
- **Prettier**: Code formatting
- **TypeScript**: Type checking
- **Husky**: Git hooks for quality checks

## MCP Integration Setup

### 1. Install MCP Package
```bash
yarn add -D @modelcontextprotocol/server-filesystem
```

### 2. Create MCP Configuration
Create `.cursor/mcp.json` in project root:
```json
{
  "mcpServers": {
    "avent-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "avent-standards": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./standards"]
    }
  }
}
```

### 3. Test MCP Integration
1. Restart Cursor AI
2. Test with prompts like:
   - "What is our current project status?"
   - "Show me our development priorities"
   - "What are our coding standards?"

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Kill process using port 3000
lsof -ti:3000 | xargs kill -9
```

#### Database Connection Issues
- Verify environment variables
- Check Supabase project status
- Confirm network connectivity

#### Build Errors
- Clear node_modules: `rm -rf node_modules && yarn install`
- Clear Next.js cache: `rm -rf .next`

#### MCP Integration Issues
- Ensure package is installed: `yarn list @modelcontextprotocol/server-filesystem`
- Check JSON syntax in `.cursor/mcp.json`
- Restart Cursor AI completely

## Next Steps

After successful setup:

1. **Review Architecture**: See `../architecture/overview.md`
2. **Check Status**: See `../status/progress.yaml`
3. **Start Development**: Begin with current priorities
4. **Run Tests**: Ensure everything is working

## Related Documentation

- [Project Overview](../index.md)
- [System Architecture](../architecture/overview.md)
- [Current Status](../status/progress.yaml)
- [Coding Standards](../../standards/coding.md)