# 🚀 Web Project Boilerplate Setup Guide

**Complete guide for setting up a new web project using this AI-friendly documentation boilerplate**

---

## 🎯 **What This Boilerplate Provides**

This boilerplate gives you a complete AI-friendly documentation structure for modern web projects with:

- **📋 AI-Optimized Architecture** - Single source of truth with machine-readable data
- **🤖 Advanced AI Integration** - MCP integration with context injection patterns
- **📚 Structured Documentation** - Clear hierarchy with progressive disclosure
- **🔧 Modern Tech Stack** - Next.js, TypeScript, modern tooling
- **📊 Machine-Readable Status** - YAML-based progress tracking

---

## 🚀 **Quick Setup (5 Minutes)**

### **Step 1: Copy Boilerplate**
```bash
# Copy the boilerplate to your new project
cp -r boilerplate/web-project/* your-new-project/

# Or if you're starting fresh
mkdir your-new-project
cp -r boilerplate/web-project/* your-new-project/
cd your-new-project
```

### **Step 2: Customize Project Name**
```bash
# Replace all instances of [Project Name] with your actual project name
find . -type f -name "*.md" -exec sed -i 's/\[Project Name\]/Your Project Name/g' {} \;
```

### **Step 3: Update Tech Stack**
Edit the following files to match your technology choices:
- `README.md` - Update tech stack section
- `.ai/context.yaml` - Update project metadata
- `docs/architecture/overview.md` - Update architecture details
- `standards/coding.md` - Update tech stack references

### **Step 4: Initialize Git**
```bash
git init
git add .
git commit -m "Initial commit: AI-friendly documentation structure"
```

---

## 🔧 **Detailed Customization**

### **1. Update Project Information**

#### **`.ai/context.yaml`**
- Replace `[Project Name]` with your project name
- Update tech stack details
- Set current development phase
- Define completion percentage and priorities

#### **`docs/index.md`**
- Update project overview and purpose
- Customize current status and priorities
- Update tech stack and architecture details
- Modify documentation structure if needed

#### **`docs/status/progress.yaml`**
- Update project name and current phase
- Customize completed tasks based on your status
- Update current priorities and roadmap
- Modify success metrics for your project

### **2. Customize Technology Stack**

#### **Backend & Database**
```markdown
- **Backend**: [Your Choice] (Supabase, AWS, Firebase, etc.)
- **Database**: [Your Choice] (PostgreSQL, MongoDB, etc.)
- **API**: [Your Choice] (GraphQL, REST, tRPC, etc.)
```

#### **Authentication & Security**
```markdown
- **Authentication**: [Your Choice] (Supabase Auth, NextAuth, Auth0, etc.)
- **Security**: [Your Features] (JWT, RLS, CORS, etc.)
```

#### **Deployment & Infrastructure**
```markdown
- **Hosting**: [Your Choice] (Vercel, Netlify, AWS, etc.)
- **CI/CD**: [Your Choice] (GitHub Actions, GitLab CI, etc.)
```

### **3. Update Business Logic**

#### **`standards/business-rules.md`**
- Define your business domain model
- Specify core entities and relationships
- Document business rules and requirements
- Create feature roadmap

#### **`standards/patterns.md`**
- Customize architectural patterns for your project
- Update component patterns and state management
- Modify error handling and performance patterns

### **4. Customize MCP Integration**

#### **MCP Configuration**
Update `.ai/mcp-config.json` with your project structure:
```json
{
  "mcpServers": {
    "your-project-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "your-project-standards": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./standards"]
    }
  }
}
```

---

## 📁 **File Structure Overview**

The boilerplate provides a complete documentation structure following AI-friendly architecture:

```
your-new-project/
├── README.md                    # Main project README (customize this)
├── SETUP_GUIDE.md              # This setup guide (delete after setup)
├── .ai/                        # AI-specific configuration
│   ├── context.yaml            # Master context file for AI agents
│   ├── mcp-config.json        # MCP server configuration
│   └── agent-instructions.md  # AI behavior guidelines
├── docs/                       # Project documentation
│   ├── index.md               # Single entry point
│   ├── status/                # Current state only
│   │   ├── progress.yaml      # Machine-readable status
│   │   └── priorities.md      # Current priorities only
│   ├── architecture/          # Technical implementation
│   │   ├── overview.md        # System architecture
│   │   ├── database.md        # Data model
│   │   └── api.md             # API specifications
│   └── guides/                # How-to documentation
│       ├── setup.md           # Environment setup
│       └── deployment.md      # Deployment processes
├── standards/                  # Immutable standards
│   ├── coding.md              # Code conventions
│   ├── patterns.md            # Architectural patterns
│   └── business-rules.md      # Domain logic
└── [other files]              # Additional documentation
```

---

## 🎯 **Customization Checklist**

### **Essential Updates**
- [ ] **Project Name**: Replace `[Project Name]` throughout
- [ ] **Tech Stack**: Update with your technology choices
- [ ] **Business Logic**: Define your domain model
- [ ] **Current Status**: Update progress.yaml
- [ ] **MCP Configuration**: Customize server names

### **Recommended Updates**
- [ ] **Architecture**: Update with your system design
- [ ] **Deployment**: Customize deployment strategy
- [ ] **Testing**: Update testing approach
- [ ] **Security**: Define security requirements
- [ ] **Performance**: Set performance targets

### **Optional Updates**
- [ ] **Design System**: Customize UI/UX guidelines
- [ ] **Internationalization**: Add language support details
- [ ] **Monitoring**: Define observability strategy
- [ ] **Compliance**: Add regulatory requirements

---

## 🚀 **Next Steps After Setup**

### **1. Initialize Development Environment**
```bash
# Install dependencies
yarn install

# Set up environment variables
cp env.example .env

# Initialize database (if applicable)
yarn db:setup

# Start development server
yarn dev
```

### **2. Set Up MCP Integration**
```bash
# Install MCP package
yarn add -D @modelcontextprotocol/server-filesystem

# Create MCP configuration
mkdir -p .cursor
# Copy the MCP configuration from .ai/mcp-config.json

# Test MCP integration
# Restart Cursor AI and test with prompts
```

### **3. Begin Development**
1. **Update Status**: Modify `docs/status/progress.yaml` with your current status
2. **Set Priorities**: Define your development priorities and roadmap
3. **Create Components**: Start building your application following the established patterns
4. **Document Progress**: Keep documentation updated as you progress

---

## 🔧 **Troubleshooting Common Issues**

### **MCP Integration Issues**
- **Package not found**: Ensure `@modelcontextprotocol/server-filesystem` is installed
- **Configuration errors**: Check JSON syntax in `.ai/mcp-config.json`
- **AI not recognizing**: Restart Cursor AI completely after configuration

### **Documentation Sync Issues**
- **Inconsistent status**: Update `docs/status/progress.yaml` first
- **Broken references**: Use single reference pattern from architecture
- **Redundant information**: Follow single source of truth principle

### **Customization Issues**
- **Tech stack mismatch**: Update all relevant files consistently
- **Business logic confusion**: Start with `standards/business-rules.md`
- **Pattern inconsistency**: Follow `standards/patterns.md` standards

---

## 📚 **Additional Resources**

### **Documentation Templates**
- **API Documentation**: Use `docs/architecture/api.md` as template
- **Database Setup**: Customize `docs/architecture/database.md` for your database
- **Deployment Guide**: Update `docs/guides/deployment.md` for your platform

### **Development Standards**
- **Coding Standards**: Follow `standards/coding.md` for consistency
- **Architectural Patterns**: Use `standards/patterns.md` for design patterns
- **Business Rules**: Follow `standards/business-rules.md` for domain logic

### **External Resources**
- **MCP Documentation**: https://modelcontextprotocol.io
- **Next.js Documentation**: https://nextjs.org/docs
- **Your Backend Docs**: [Your Backend Documentation URL]

---

## ✅ **Setup Complete!**

**Your project now has:**
- ✅ **AI-friendly documentation structure** with single source of truth
- ✅ **Advanced MCP integration** for AI development assistance
- ✅ **Machine-readable status** for efficient AI consumption
- ✅ **Clear document hierarchy** with progressive disclosure
- ✅ **Eliminated redundancy** and circular references

**Next steps:**
1. **Customize** the templates for your specific project
2. **Set up MCP integration** for AI assistance
3. **Begin development** following established patterns
4. **Maintain documentation** as you progress

---

**Remember**: This boilerplate follows AI-friendly architecture principles. Start with the basics and enhance gradually as your project grows.



