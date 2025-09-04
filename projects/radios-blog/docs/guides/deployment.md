# Deployment Guide - Radios Blog

> **AI Context**: This is the single source of truth for deployment processes.
> For current status: see `../status/progress.yaml`
> For system overview: see `../architecture/overview.md`

## Deployment Overview

Radios Blog is designed for deployment on Vercel (MVP) with future migration to AWS for production scaling and content management capabilities.

## Deployment Environments

### Development
- **URL**: [dev-url]
- **Purpose**: Feature development and testing
- **Auto-deploy**: On push to `develop` branch

### Staging
- **URL**: [staging-url]
- **Purpose**: Pre-production testing
- **Auto-deploy**: On push to `staging` branch

### Production
- **URL**: [production-url]
- **Purpose**: Live application
- **Auto-deploy**: On push to `main` branch

## Current MVP Deployment (Vercel + Supabase)

### Vercel Configuration

Create `vercel.json` in project root:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/next"
    }
  ],
  "env": {
    "NEXT_PUBLIC_APP_URL": "@app_url",
    "NEXT_PUBLIC_APP_NAME": "@app_name"
  },
  "functions": {
    "app/api/radios/route.ts": {
      "maxDuration": 30
    },
    "app/api/blog/route.ts": {
      "maxDuration": 30
    },
    "app/api/postcards/route.ts": {
      "maxDuration": 30
    }
  }
}
```

### Environment Variables

Set in Vercel dashboard:

```env
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# Database URLs
DATABASE_URL=your_supabase_database_url
DIRECT_URL=your_supabase_direct_url

# App Configuration
NEXT_PUBLIC_APP_URL=https://your-domain.vercel.app
NEXT_PUBLIC_APP_NAME=Radios Blog

# Security
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=https://your-domain.vercel.app

# Image Storage
NEXT_PUBLIC_IMAGE_STORAGE_URL=your_image_storage_url
```

### Deployment Process

#### Automatic Deployment
1. Connect GitHub repository to Vercel
2. Configure build settings:
   - **Framework Preset**: Next.js
   - **Build Command**: `yarn build`
   - **Output Directory**: `.next`
   - **Install Command**: `yarn install --frozen-lockfile`

#### Manual Deployment
```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Deploy
vercel --prod
```

## CI/CD Pipeline

### GitHub Actions Workflow

Create `.github/workflows/ci.yml`:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, dev]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
        cache: 'yarn'
    
    - name: Install dependencies
      run: yarn install --frozen-lockfile
    
    - name: Run linting
      run: yarn lint
    
    - name: Run type checking
      run: yarn type-check
    
    - name: Run tests
      run: yarn test:ci
    
    - name: Run E2E tests
      run: yarn test:e2e
    
    - name: Build application
      run: yarn build
      env:
        NEXT_PUBLIC_SUPABASE_URL: ${{ secrets.NEXT_PUBLIC_SUPABASE_URL }}
        NEXT_PUBLIC_SUPABASE_ANON_KEY: ${{ secrets.NEXT_PUBLIC_SUPABASE_ANON_KEY }}

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Deploy to Vercel
      uses: amondnet/vercel-action@v25
      with:
        vercel-token: ${{ secrets.VERCEL_TOKEN }}
        vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
        vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
        vercel-args: '--prod'
```

### Required Secrets
- `VERCEL_TOKEN`: Vercel deployment token
- `VERCEL_ORG_ID`: Vercel organization ID
- `VERCEL_PROJECT_ID`: Vercel project ID
- `NEXT_PUBLIC_SUPABASE_URL`: Supabase project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`: Supabase anonymous key

## Future AWS Migration

### Target Architecture
- **Frontend**: AWS Amplify or CloudFront + S3
- **Backend**: AWS Lambda + API Gateway
- **Database**: Amazon RDS (PostgreSQL)
- **Storage**: S3 + CloudFront for images
- **Auth**: AWS Cognito
- **CDN**: CloudFront
- **Monitoring**: CloudWatch + X-Ray

### Migration Strategy

#### Phase 1: Database Migration
1. Set up Amazon RDS PostgreSQL
2. Configure read replicas
3. Data migration from Supabase
4. Update connection strings

#### Phase 2: API Migration
1. Deploy API to AWS Lambda
2. Set up API Gateway
3. Configure authentication
4. Update frontend API endpoints

#### Phase 3: Storage Migration
1. Set up S3 buckets for images
2. Configure CloudFront distribution
3. Migrate existing images
4. Update image URLs

#### Phase 4: Frontend Migration
1. Deploy to AWS Amplify
2. Configure custom domain
3. Set up SSL certificates
4. Update DNS records

## Security Configuration

### Supabase Security
- **Row Level Security (RLS)**: Enabled on all tables
- **API Security**: Rate limiting and CORS configuration
- **Input Validation**: Zod schema validation

### Vercel Security
- **Security Headers**: Configured in `next.config.ts`
- **HTTPS**: Automatic SSL certificates
- **Environment Variables**: Secure storage in Vercel dashboard

## Monitoring & Analytics

### Vercel Analytics
```bash
yarn add @vercel/analytics
```

Add to `app/layout.tsx`:
```tsx
import { Analytics } from '@vercel/analytics/react'

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  )
}
```

### Error Monitoring
```bash
yarn add @sentry/nextjs
```

Configure Sentry for error tracking and performance monitoring.

## Health Checks

### Application Health
```typescript
// app/api/health/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  try {
    // Check database connection
    // Check external services
    // Check application status
    
    return NextResponse.json({
      status: 'healthy',
      timestamp: new Date().toISOString(),
      version: process.env.npm_package_version,
    })
  } catch (error) {
    return NextResponse.json(
      { status: 'unhealthy', error: error.message },
      { status: 500 }
    )
  }
}
```

## Content Management

### Image Storage
- **Development**: Local file system
- **MVP**: Supabase Storage
- **Production**: S3 + CloudFront

### Content Updates
- **Blog Posts**: Admin panel for content management
- **Radio Listings**: Seller dashboard for updates
- **Postcards**: Curated collection management

## Related Documentation

- [System Architecture](../architecture/overview.md)
- [Database Setup](../architecture/database.md)
- [Current Status](../status/progress.yaml)
- [Environment Setup](setup.md)