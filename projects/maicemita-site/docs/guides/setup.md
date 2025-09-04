# Environment Setup Guide - Maicemita Site

> **AI Context**: This is the single source of truth for environment setup.
> For current status: see `../status/progress.yaml`
> For project overview: see `../index.md`

## Prerequisites

Before setting up Maicemita Site, ensure you have the following installed:

- **Node.js**: Version 18+ (recommended: LTS)
- **Yarn**: Package manager (required)
- **Git**: Version control
- **Vercel CLI**: For deployment (optional but recommended)
- **Cursor AI** (optional): For MCP integration

## Quick Setup (5 minutes)

### 1. Clone the Repository
```bash
git clone [your-repository-url]
cd maicemita-site
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
# Vercel Configuration
VERCEL_URL=your_vercel_url
VERCEL_ENV=development

# Database Configuration (Vercel Postgres)
POSTGRES_URL=your_postgres_url
POSTGRES_PRISMA_URL=your_postgres_prisma_url
POSTGRES_URL_NON_POOLING=your_postgres_url_non_pooling
POSTGRES_USER=your_postgres_user
POSTGRES_HOST=your_postgres_host
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DATABASE=your_postgres_database

# Email Configuration (for order notifications)
SMTP_HOST=your_smtp_host
SMTP_PORT=your_smtp_port
SMTP_USER=your_smtp_user
SMTP_PASS=your_smtp_password
EMAIL_FROM=orders@maicemita.com

# Analytics (optional)
NEXT_PUBLIC_GA_ID=your_google_analytics_id

# App Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=Maicemita
NEXT_PUBLIC_BUSINESS_EMAIL=contact@maicemita.com
NEXT_PUBLIC_BUSINESS_PHONE=+1234567890
```

### Database Setup

1. **Create Vercel Postgres Database**: 
   - Go to [vercel.com/dashboard](https://vercel.com/dashboard)
   - Create new project or add database to existing project
   - Note your database connection details

2. **Run Database Migrations** (when implemented):
   ```bash
   npx prisma migrate dev
   ```

3. **Seed Database** (when implemented):
   ```bash
   npx prisma db seed
   ```

4. **Verify Connection**:
   ```bash
   npx prisma studio
   ```

### Vercel Setup

1. **Install Vercel CLI**:
   ```bash
   npm i -g vercel
   ```

2. **Login to Vercel**:
   ```bash
   vercel login
   ```

3. **Link Project**:
   ```bash
   vercel link
   ```

4. **Deploy to Preview**:
   ```bash
   vercel
   ```

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
    "maicemita-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "maicemita-standards": {
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
   - "How do I set up the development environment?"

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
    "format": "Format code with Prettier",
    "format:check": "Check code formatting",
    "analyze": "Analyze bundle size",
    "clean": "Clean build artifacts"
  }
}
```

### Code Quality Tools

- **ESLint**: Code linting and formatting
- **Prettier**: Code formatting
- **TypeScript**: Type checking
- **Husky**: Git hooks for quality checks
- **Lint-staged**: Pre-commit hooks

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# Kill process using port 3000
lsof -ti:3000 | xargs kill -9

# Or use a different port
yarn dev -- -p 3001
```

#### Database Connection Issues
- Verify environment variables are correct
- Check Vercel Postgres service status
- Confirm network connectivity
- Verify database credentials and permissions

#### Build Errors
- Clear node_modules: `rm -rf node_modules && yarn install`
- Clear Next.js cache: `rm -rf .next`
- Clear package manager cache: `yarn cache clean`
- Check for TypeScript errors: `yarn type-check`

#### Vercel Deployment Issues
- Verify environment variables in Vercel dashboard
- Check build logs in Vercel dashboard
- Ensure all dependencies are in package.json
- Verify build command in vercel.json

#### MCP Integration Issues
- Ensure package is installed: `yarn list @modelcontextprotocol/server-filesystem`
- Check JSON syntax in `.cursor/mcp.json`
- Restart Cursor AI completely
- Verify file paths in MCP configuration

#### Performance Issues
- Check bundle size: `yarn analyze`
- Review network tab in browser dev tools
- Monitor memory usage
- Check for memory leaks in components

### Environment-Specific Issues

#### Development Environment
- Hot reload not working: Restart dev server
- Styling issues: Check TailwindCSS configuration
- API calls failing: Verify environment variables

#### Production Environment
- Build failing: Check environment variables in Vercel
- Runtime errors: Review error logs in Vercel dashboard
- Performance issues: Enable Vercel Analytics

## Security Considerations

### Environment Variables
- Never commit `.env.local` to version control
- Use different secrets for different environments
- Rotate secrets regularly
- Use environment-specific configurations

### Database Security
- Enable SSL connections
- Use connection pooling
- Implement proper access controls
- Regular security audits

### API Security
- Implement rate limiting
- Use HTTPS in production
- Validate all inputs
- Implement proper CORS policies

## Next Steps

After successful setup:

1. **Review Architecture**: See `../architecture/overview.md`
2. **Check Status**: See `../status/progress.yaml`
3. **Start Development**: Begin with current priorities
4. **Run Tests**: Ensure everything is working
5. **Set up MCP**: Configure AI development assistance
6. **Review Standards**: Familiarize yourself with coding standards

## Related Documentation

- [Project Overview](../index.md)
- [System Architecture](../architecture/overview.md)
- [Current Status](../status/progress.yaml)
- [Coding Standards](../../standards/coding.md)
- [MCP Integration Guide](../../.ai/agent-instructions.md)
