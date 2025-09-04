# Sebastián Pereira Rivero - Personal Site

> **AI Context**: This is the single source of truth for the personal site project.
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`

## Project Overview

Personal landing page showcasing Sebastián's professional experience as a Senior Frontend Developer. The site serves as an interactive resume with project portfolio, skills showcase, and contact form.

## Quick Start

1. **Setup**: See `docs/guides/setup.md` for environment setup
2. **Status**: Check `docs/status/progress.yaml` for current progress
3. **Architecture**: Review `docs/architecture/overview.md` for technical details
4. **Design System**: See `docs/design-system/components.md` for shadcn/ui usage

## Tech Stack

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS
- **UI Components**: shadcn/ui + Radix UI primitives
- **Styling**: TailwindCSS utility-first approach
- **Forms**: React Hook Form + Zod validation
- **Contact**: Next.js API Routes for email sending
- **Deployment**: Vercel (recommended)

## Project Structure

```
personal-site/
├── .ai/                    # AI-specific configuration
├── docs/                   # Project documentation
│   ├── status/            # Current state only
│   ├── architecture/      # Technical implementation
│   ├── design-system/     # UI/UX guidelines and components
│   └── guides/            # How-to documentation
├── standards/              # Immutable standards
└── [src/]                 # Application source code
```

## Site Sections

### 1. Hero Section
- Name and professional title
- Brief professional summary
- Call-to-action buttons (Download CV, Contact)

### 2. About/Profile Section
- Professional summary from CV
- Key skills and expertise
- Professional philosophy

### 3. Experience Timeline
- Chronological work experience
- Key achievements and technologies
- Company logos and details

### 4. Skills Showcase
- Technical skills organized by category
- Proficiency levels and certifications
- Interactive skill visualization

### 5. Project Gallery
- Linear clone (project management tool)
- Avent Properties (lease accounting SaaS)
- Train UY (transportation platform)
- RPA automation projects
- Other relevant projects

### 6. Contact Form
- Name, email, subject, message fields
- Form validation with Zod
- Email submission to spereirarivero93@gmail.com
- Success/error feedback

### 7. Downloadable CV
- PDF download option
- Multiple format support
- Professional styling

## Related Documentation

- [Current Status](docs/status/progress.yaml)
- [Architecture](docs/architecture/overview.md)
- [Design System](docs/design-system/components.md)
- [Setup Guide](docs/guides/setup.md)
- [Coding Standards](standards/coding.md)

## Getting Help

- Start with this file for project overview
- Check status files for current progress
- Refer to specific guides for implementation details
- Follow coding standards for development
- Use design system for consistent UI components

