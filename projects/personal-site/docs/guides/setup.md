# Environment Setup Guide

> **AI Context**: This is the single source of truth for environment setup.
> For current status: see `../status/progress.yaml`
> For project overview: see `../index.md`

## Prerequisites

Before setting up Sebastián's Personal Site, ensure you have the following installed:

- **Node.js**: Version 18+ (recommended: LTS)
- **npm** or **yarn**: Package manager (yarn recommended)
- **Git**: Version control
- **Code Editor**: VS Code with Cursor AI extension recommended

## Quick Setup (10 minutes)

### 1. Create Next.js Project
```bash
# Create new Next.js project with TypeScript
npx create-next-app@latest personal-site --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# Navigate to project directory
cd personal-site
```

### 2. Install shadcn/ui
```bash
# Initialize shadcn/ui
npx shadcn@latest init

# Choose these options:
# - Style: Default
# - Base color: Slate
# - CSS variables: Yes
# - React Server Components: Yes
# - Components directory: @/components/ui
# - Utils directory: @/lib/utils
# - Include example components: No
```

### 3. Install Required shadcn/ui Components
```bash
# Install components needed for the site
npx shadcn@latest add card button badge avatar separator progress input textarea label alert sheet aspect-ratio
```

### 4. Install Additional Dependencies
```bash
# Form handling and validation
yarn add react-hook-form @hookform/resolvers zod

# Icons
yarn add lucide-react

# Email handling (for contact form)
yarn add nodemailer @types/nodemailer

# Optional: Animation library
yarn add framer-motion
```

### 5. Environment Configuration
```bash
# Create environment file
cp .env.example .env.local
```

Fill in your environment variables:
```env
# Email Configuration (for contact form)
EMAIL_SERVER_HOST=smtp.gmail.com
EMAIL_SERVER_PORT=587
EMAIL_SERVER_USER=your-email@gmail.com
EMAIL_SERVER_PASSWORD=your-app-password

# Site Configuration
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NEXT_PUBLIC_SITE_NAME="Sebastián Pereira Rivero - Personal Site"
```

### 6. Start Development Server
```bash
yarn dev
```

Your application should now be running at `http://localhost:3000`

## Detailed Setup

### Project Structure Setup

Create the following directory structure:
```bash
mkdir -p src/components/{sections,forms,layout}
mkdir -p src/lib
mkdir -p src/types
mkdir -p src/data
mkdir -p public/{images,documents}
```

### shadcn/ui Configuration

Ensure your `components.json` is properly configured:
```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "default",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "src/app/globals.css",
    "baseColor": "slate",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils"
  }
}
```

### TailwindCSS Configuration

Update `tailwind.config.ts` for custom colors and animations:
```typescript
import type { Config } from "tailwindcss"

const config: Config = {
  darkMode: ["class"],
  content: [
    './pages/**/*.{ts,tsx}',
    './components/**/*.{ts,tsx}',
    './app/**/*.{ts,tsx}',
    './src/**/*.{ts,tsx}',
  ],
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
}

export default config
```

### Contact Form API Setup

Create the contact form API route at `src/app/api/contact/route.ts`:
```typescript
import { NextRequest, NextResponse } from 'next/server'
import nodemailer from 'nodemailer'
import { z } from 'zod'

const contactSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email address'),
  subject: z.string().min(5, 'Subject must be at least 5 characters'),
  message: z.string().min(10, 'Message must be at least 10 characters'),
})

export async function POST(request: NextRequest) {
  try {
    const body = await request.json()
    const { name, email, subject, message } = contactSchema.parse(body)

    // Create transporter
    const transporter = nodemailer.createTransporter({
      host: process.env.EMAIL_SERVER_HOST,
      port: parseInt(process.env.EMAIL_SERVER_PORT || '587'),
      secure: false,
      auth: {
        user: process.env.EMAIL_SERVER_USER,
        pass: process.env.EMAIL_SERVER_PASSWORD,
      },
    })

    // Send email
    await transporter.sendMail({
      from: process.env.EMAIL_SERVER_USER,
      to: 'spereirarivero93@gmail.com',
      subject: `Contact Form: ${subject}`,
      html: `
        <h2>New Contact Form Submission</h2>
        <p><strong>Name:</strong> ${name}</p>
        <p><strong>Email:</strong> ${email}</p>
        <p><strong>Subject:</strong> ${subject}</p>
        <p><strong>Message:</strong></p>
        <p>${message}</p>
      `,
    })

    return NextResponse.json({ success: true })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json(
        { error: 'Validation failed', details: error.errors },
        { status: 400 }
      )
    }

    console.error('Contact form error:', error)
    return NextResponse.json(
      { error: 'Failed to send message' },
      { status: 500 }
    )
  }
}
```

### Data Files Setup

Create sample data files for easy content management:

#### `src/data/experience.ts`
```typescript
export interface Experience {
  id: string
  title: string
  company: string
  period: string
  description: string
  technologies: string[]
  achievements: string[]
}

export const experiences: Experience[] = [
  {
    id: 'mimiquate',
    title: 'Software Engineer',
    company: 'Mimiquate',
    period: 'Jan 2024 - Apr 2024',
    description: 'Built a project management tool (Linear clone) using Elixir + Phoenix.',
    technologies: ['Elixir', 'Phoenix', 'PostgreSQL', 'JavaScript'],
    achievements: [
      'Mentored junior developers and led code reviews & tech talks',
      'Enhanced collaboration practices within a fast-moving team'
    ]
  },
  // Add more experiences from CV
]
```

#### `src/data/projects.ts`
```typescript
export interface Project {
  id: string
  title: string
  description: string
  technologies: string[]
  image: string
  demoUrl?: string
  sourceUrl?: string
  featured: boolean
}

export const projects: Project[] = [
  {
    id: 'linear-clone',
    title: 'Linear Clone - Project Management Tool',
    description: 'Built a project management tool using Elixir + Phoenix, featuring task management, team collaboration, and project tracking.',
    technologies: ['Elixir', 'Phoenix', 'PostgreSQL', 'JavaScript', 'React'],
    image: '/images/projects/linear-clone.jpg',
    demoUrl: 'https://demo.example.com',
    sourceUrl: 'https://github.com/username/linear-clone',
    featured: true
  },
  // Add more projects
]
```

## Development Workflow

### Available Scripts

```json
{
  "scripts": {
    "dev": "Next.js development server",
    "build": "Production build",
    "start": "Production server",
    "lint": "Code linting",
    "type-check": "TypeScript type checking",
    "format": "Format code with Prettier"
  }
}
```

### Code Quality Tools

- **ESLint**: Code linting and formatting
- **Prettier**: Code formatting
- **TypeScript**: Type checking
- **Husky**: Git hooks for quality checks (optional)

## Troubleshooting

### Common Issues

#### shadcn/ui Components Not Found
```bash
# Reinstall components
npx shadcn@latest add [component-name]

# Check components.json configuration
cat components.json
```

#### Contact Form Not Working
- Verify environment variables are set correctly
- Check email server credentials
- Ensure nodemailer is installed
- Check browser console for errors

#### Styling Issues
- Verify TailwindCSS is properly configured
- Check globals.css includes shadcn/ui CSS variables
- Ensure components are imported correctly

## Next Steps

After successful setup:

1. **Review Architecture**: See `../architecture/overview.md`
2. **Check Design System**: See `../design-system/components.md`
3. **Start Building**: Begin with Hero section
4. **Test Contact Form**: Verify email functionality works

## Related Documentation

- [Project Overview](../index.md)
- [System Architecture](../architecture/overview.md)
- [Design System](../design-system/components.md)
- [Current Status](../status/progress.yaml)

