# 🚀 Sebastián Pereira Rivero - Personal Site

**Professional landing page showcasing Sebastián's experience as a Senior Frontend Developer**

---

## 📚 **Documentation & Getting Started**

**🎯 For complete project documentation, start with `docs/index.md`**

This root README provides quick setup instructions. For comprehensive documentation including:
- Current project status and progress
- Design system and component guidelines
- Development standards and patterns
- MCP integration setup
- Technical implementation details

**→ Go to `docs/index.md` for the complete picture**

## 🛠️ Tech Stack

- **Frontend**: Next.js 15 (App Router) + TypeScript + React 19
- **Styling**: TailwindCSS + shadcn/ui + Radix UI
- **Forms**: React Hook Form + Zod validation
- **Contact**: Next.js API Routes + Nodemailer
- **Icons**: Lucide React
- **Package Manager**: Yarn
- **Deployment**: Vercel (recommended)

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ 
- Yarn package manager

### 1. Clone and Install
```bash
git clone <repository-url>
cd personal-site
yarn install
```

### 2. Environment Setup
```bash
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

### 3. Development
```bash
# Start development server
yarn dev

# Build for production
yarn build

# Type checking
yarn type-check

# Linting
yarn lint
```

## 📁 Project Structure

```
personal-site/
├── .ai/                    # AI-specific configuration
├── docs/                   # Project documentation
│   ├── status/            # Current state only
│   ├── architecture/      # Technical implementation
│   ├── design-system/     # UI/UX guidelines and components
│   └── guides/            # How-to documentation
├── standards/              # Immutable standards
├── src/                   # Application source code
│   ├── app/              # Next.js App Router pages
│   ├── components/       # React components
│   │   ├── sections/    # Site section components
│   │   ├── forms/       # Form components
│   │   ├── layout/      # Layout components
│   │   └── ui/          # shadcn/ui components
│   ├── lib/             # Utilities and configurations
│   ├── types/           # TypeScript type definitions
│   └── data/            # Static data files
└── public/              # Static assets
```

## 🎯 Site Sections

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

### 6. Contact Form
- Name, email, subject, message fields
- Form validation with Zod
- Email submission to spereirarivero93@gmail.com
- Success/error feedback

### 7. Downloadable CV
- PDF download option
- Multiple format support
- Professional styling

## 🔐 Contact Form

The contact form sends emails directly to `spereirarivero93@gmail.com` using:
- **Frontend**: React Hook Form + Zod validation
- **Backend**: Next.js API Routes
- **Email Service**: Nodemailer with SMTP

## 🎨 Design System

Built with **shadcn/ui** components for:
- **Consistency**: Professional, modern design language
- **Accessibility**: Built-in ARIA support and keyboard navigation
- **Responsiveness**: Mobile-first design approach
- **Customization**: Easy theming and component extension

## 🧪 Testing Strategy

- **Unit Tests**: Jest + React Testing Library
- **Component Testing**: Test all interactive elements
- **Form Validation**: Test contact form functionality
- **Accessibility**: Test with screen readers and keyboard navigation

## 📦 Available Scripts

```bash
# Development
yarn dev              # Start development server
yarn build            # Build for production
yarn start            # Start production server

# Code Quality
yarn lint             # Run ESLint
yarn type-check       # TypeScript type checking
yarn format           # Format with Prettier

# Testing
yarn test             # Run unit tests
yarn test:watch       # Run tests in watch mode
```

## 🚀 Deployment

### Vercel (Recommended)
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Set environment variables in Vercel dashboard
```

### Environment Variables for Production
- `EMAIL_SERVER_HOST`: SMTP server host
- `EMAIL_SERVER_PORT`: SMTP server port
- `EMAIL_SERVER_USER`: SMTP username
- `EMAIL_SERVER_PASSWORD`: SMTP password

## 📚 Documentation Structure

**🎯 Complete documentation is organized in the `/docs` folder**

### **Quick Navigation**
- **`docs/index.md`** - **COMPLETE PROJECT OVERVIEW** (Start here!)
- **`docs/status/progress.yaml`** - Current project status and progress
- **`docs/architecture/overview.md`** - System architecture overview
- **`docs/design-system/components.md`** - UI/UX guidelines and shadcn/ui usage
- **`docs/guides/setup.md`** - Environment setup guide

### **For AI Assistants & Developers**
- **`standards/`** - Development standards and patterns
- **`.ai/`** - AI-specific configuration and context

**→ See `docs/index.md` for complete documentation structure and navigation**

## 🤖 AI Development Assistance

### **MCP Integration**
We use Model Context Protocol (MCP) to enhance AI development assistance with 2-server setup (docs + standards).

**→ See `docs/index.md` for complete MCP setup and testing instructions**

## 🤝 Contributing

1. **Follow coding standards** in `standards/coding.md`
2. **Write tests** for new features
3. **Use conventional commit messages**
4. **Create feature branches** from `main`
5. **Submit pull requests** for review
6. **Update documentation** when making changes

**→ See `docs/guides/setup.md` for development workflow**

## 📄 License

[Your License]

---

**Built with ❤️ by Sebastián Pereira Rivero**

---

## 🔄 Documentation Status

- **Last Updated**: December 19, 2024
- **Documentation**: ✅ **CONSOLIDATED** - Single source of truth in `docs/index.md`
- **MCP Integration**: ✅ Ready for implementation
- **Next Review**: Monthly or when major changes occur

**📚 For complete documentation, start with `docs/index.md`**

---

## 🎯 **Quick Start**

1. **Read this file** for project overview and setup
2. **Go to `docs/index.md`** for complete documentation
3. **Check `docs/status/progress.yaml`** for current status
4. **Follow `docs/guides/setup.md`** for development workflow
5. **Review `docs/design-system/components.md`** for UI guidelines

