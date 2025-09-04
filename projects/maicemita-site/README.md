# 🧁 Maicemita Site

**Beautiful e-commerce site for homemade alfajores de maicena - Sweet pastries made with love**

---

## 🎯 **What This Project Is**

Maicemita Site is a simple, beautiful e-commerce website to promote and sell homemade alfajores de maicena (sweet pastry). The site showcases different box sizes and flavors, providing an easy way for customers to discover and order these delicious homemade desserts.

### **Business Mission**
Create a fast, mobile-friendly website that beautifully showcases homemade alfajores and makes it easy for customers to place orders, helping grow the Maicemita business.

---

## 🌟 **Key Features**

- ✅ **Beautiful Product Showcase** - High-quality images and descriptions
- ✅ **Box Size Selection** - Small, medium, and large box options
- ✅ **Flavor Selection** - Dulce de leche, membrillo, and batata flavors
- ✅ **Simple Ordering** - Easy contact form for orders
- ✅ **Mobile-First Design** - Optimized for mobile users
- ✅ **Fast Loading** - SEO optimized with static site generation

---

## 🚀 **Quick Start**

### **Development Setup**
```bash
# Clone and setup
git clone [repository-url]
cd maicemita-site
yarn install

# Environment setup
cp .env.example .env.local
# Edit .env.local with your configuration

# Start development
yarn dev
```

### **AI Development Assistance**
This project uses MCP (Model Context Protocol) for enhanced AI development:

1. **Install MCP Package**: `yarn add -D @modelcontextprotocol/server-filesystem`
2. **Restart Cursor AI** after configuration
3. **Test with prompts** like:
   - "What is our current project status?"
   - "Show me our development priorities"
   - "Create a product gallery component"

---

## 🏗️ **Tech Stack**

- **Frontend**: Next.js 15 + TypeScript + TailwindCSS + shadcn/ui
- **Backend**: Next.js API Routes
- **Database**: Vercel Postgres (MVP)
- **Deployment**: Vercel with automatic deployments
- **Testing**: Jest + React Testing Library
- **Monitoring**: Vercel Analytics

---

## 📚 **Documentation**

### **Core Documentation**
- [Project Overview](docs/index.md) - Complete project overview
- [Current Status](docs/status/progress.yaml) - Machine-readable project status
- [System Architecture](docs/architecture/overview.md) - Technical architecture
- [Setup Guide](docs/guides/setup.md) - Environment setup instructions
- [Coding Standards](standards/coding.md) - Development guidelines

### **AI Context**
- [Master Context](.ai/context.yaml) - Project metadata for AI agents
- [MCP Configuration](.cursor/mcp.json) - AI development assistance setup

---

## 🎯 **Current Status**

- **Phase**: Planning & Documentation
- **Completion**: 5%
- **Next Priority**: Complete documentation structure and prepare for MVP development

### **Development Roadmap**
1. **Environment Setup** - Next.js project with TypeScript and TailwindCSS
2. **UI Implementation** - Product showcase and ordering interface
3. **Product Gallery** - Box sizes and flavors selection
4. **Contact Form** - Order processing and validation
5. **Mobile Optimization** - Responsive design and performance
6. **Deployment** - Vercel deployment with custom domain

---

## 🧁 **Product Information**

### **Box Sizes**
- **Small Box**: 6 alfajores
- **Medium Box**: 12 alfajores  
- **Large Box**: 24 alfajores

### **Available Flavors**
- **Dulce de Leche**: Classic sweet milk caramel
- **Membrillo**: Traditional quince paste
- **Batata**: Sweet potato filling

### **Business Details**
- **Business Name**: Maicemita
- **Product**: Homemade alfajores de maicena
- **Target Market**: Local customers and dessert lovers
- **Ordering**: Contact form with email/phone

---

## 🚀 **Deployment**

### **Vercel Deployment**
```bash
# Install Vercel CLI
npm i -g vercel

# Login and link project
vercel login
vercel link

# Deploy
vercel
```

### **Environment Variables**
- `POSTGRES_URL` - Database connection
- `SMTP_*` - Email configuration for orders
- `NEXT_PUBLIC_*` - Public app configuration

---

## 🤝 **Contributing**

### **Development Process**
1. **Follow Standards**: Use coding standards in `standards/coding.md`
2. **Update Documentation**: Keep docs current with changes
3. **Test Components**: Maintain test coverage
4. **Mobile-First**: Ensure mobile responsiveness

### **AI Development**
- **Use MCP Integration**: Leverage AI assistance with Cursor
- **Follow Context**: Use project context for consistent development
- **Update Status**: Keep progress tracking current

---

## 📞 **Support**

### **Getting Help**
- **Check Documentation**: Start with `docs/index.md`
- **Review Status**: See `docs/status/progress.yaml`
- **Follow Setup Guide**: Use `docs/guides/setup.md`

### **Business Contact**
- **Email**: contact@maicemita.com
- **Phone**: +1234567890
- **Business**: Homemade alfajores de maicena

---

## ✅ **Ready to Build**

**This project provides everything needed to:**
- ✅ **Create beautiful product showcase** - High-quality images and descriptions
- ✅ **Implement simple ordering** - Easy contact form for customers
- ✅ **Optimize for mobile** - Mobile-first responsive design
- ✅ **Deploy quickly** - Vercel deployment with automatic updates
- ✅ **Scale easily** - Architecture ready for future features

**Start building with AI assistance and create a beautiful e-commerce site for Maicemita! 🧁**

---

**Remember**: This project follows AI-friendly documentation architecture with single source of truth, machine-readable data, and context injection patterns for optimal AI development assistance.
