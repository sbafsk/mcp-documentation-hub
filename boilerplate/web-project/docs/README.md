# 📚 [Project Name] - Complete Documentation

**Single source of truth for the [Project Name] web application**

---

## 📚 **Documentation Philosophy**

Our documentation follows **AI-friendly architecture principles**:

- **📋 `/standards` Folder**: **Immutable Standards** - Development standards, patterns, and business rules
- **📊 `/docs` Folder**: **Project Status & Implementation** - Current project state, progress tracking, and operational guides
- **🤖 `.ai/` Folder**: **AI Configuration** - Context, MCP setup, and agent instructions

**Key Principle**: Each piece of information exists in exactly one location with clear hierarchy and machine-readable data.

**Note**: The root `README.md` provides quick project overview and setup. This `docs/index.md` is the **complete documentation overview** and single source of truth.

---

## 🎯 **Quick Start Guide**

### **Documentation Entry Points**
- **`README.md` (root)** → Quick project overview and setup instructions
- **`docs/index.md` (this file)** → **COMPLETE DOCUMENTATION OVERVIEW** - Single source of truth

### **For New Team Members**
1. **Start Here** → Read this file for complete overview
2. **Check Status** → `docs/status/progress.yaml` for current progress
3. **Learn Standards** → `standards/coding.md` for coding guidelines
4. **Understand Business** → `standards/business-rules.md` for requirements

### **For AI Assistants**
1. **Project Context** → `.ai/context.yaml` for project metadata
2. **Current Status** → `docs/status/progress.yaml` for current status
3. **Development Standards** → `standards/coding.md` for coding patterns
4. **Business Logic** → `standards/business-rules.md` for requirements
5. **MCP Integration** → `.ai/mcp-config.json` for setup

### **For Developers**
1. **Current Status** → `docs/status/progress.yaml` for priorities
2. **Setup Guides** → `docs/guides/setup.md` and `docs/guides/deployment.md`
3. **Architecture Details** → `docs/architecture/overview.md` for system design
4. **Coding Standards** → `standards/coding.md` for conventions

---

## 🏗️ **Project Overview**

### **What We're Building**
[Describe your project and its purpose]

### **Current Status**
- **Phase**: [Current Development Phase]
- **MVP Features**: [Status of core features]
- **API Implementation**: [Status of API development]
- **Documentation**: ✅ **CONSOLIDATED** - AI-friendly architecture established
- **Next Priority**: [Next development focus]

### **Tech Stack**
- **Frontend**: Next.js 15 + TypeScript + React 19 + TailwindCSS + shadcn/ui
- **Backend**: [Your Backend Choice] → Future migration path if applicable
- **API**: [Your API Choice] (REST, GraphQL, etc.)
- **State Management**: Redux Toolkit + React Query
- **Design System**: [Your Design System Description]

---

## 📁 **Documentation Structure**

### **🤖 `.ai/` - AI Configuration**

#### **Core AI Files**
- **`context.yaml`** - Master context file with project metadata
- **`mcp-config.json`** - MCP server configuration
- **`agent-instructions.md`** - AI behavior guidelines and query routing

### **📋 `/standards` - Immutable Standards**

#### **Core Standards**
- **`coding.md`** - Code conventions, naming, and best practices
- **`patterns.md`** - Architectural patterns and component design
- **`business-rules.md`** - Business logic, domain model, and requirements

### **📊 `/docs` - Project Status & Implementation**

#### **Project Status**
- **`status/progress.yaml`** - **MACHINE-READABLE STATUS** - Current progress in YAML format
- **`status/priorities.md`** - Current development priorities and deadlines
- **`index.md`** - Single entry point for all documentation

#### **Technical Implementation**
- **`architecture/overview.md`** - System architecture and design
- **`architecture/database.md`** - Database schema and data model
- **`architecture/api.md`** - API specifications and endpoints

#### **How-to Guides**
- **`guides/setup.md`** - Environment setup and configuration
- **`guides/deployment.md`** - Deployment processes and infrastructure

---

## 🚀 **Current Development Priorities**

### **Priority 1: [Current Priority]**
- [ ] [Task 1]
- [ ] [Task 2]
- [ ] [Task 3]

### **Priority 2: [Next Priority]**
- [ ] [Task 1]
- [ ] [Task 2]
- [ ] [Task 3]

### **Priority 3: [Future Priority]**
- [ ] [Task 1]
- [ ] [Task 2]
- [ ] [Task 3]

---

## 🔧 **MCP Integration Status**

### **Current Setup**
- **Status**: ✅ **READY FOR IMPLEMENTATION** - Configuration template provided
- **Configuration**: 2-server setup (docs + standards)
- **Package**: `@modelcontextprotocol/server-filesystem` ready to install

### **Configuration Template**
```json
// .ai/mcp-config.json
{
  "mcpServers": {
    "project-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "project-standards": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./standards"]
    }
  }
}
```

### **Setup Instructions**
1. Install MCP package: `yarn add -D @modelcontextprotocol/server-filesystem`
2. Copy `.ai/mcp-config.json` to `.cursor/mcp.json`
3. Restart Cursor AI
4. Test with prompts like:
   - "What is our current project status?"
   - "Show me our development priorities"
   - "What are our coding standards?"

---

## 📊 **Project Maturity Level**

### **✅ Completed (100%)**
- **Project Setup**: [What's complete]
- **Documentation**: [What's complete]
- **Basic Structure**: [What's complete]

### **🔄 In Progress**
- **[Feature 1]**: [Status]
- **[Feature 2]**: [Status]
- **[Feature 3]**: [Status]

### **📋 Coming Next**
- **[Feature 1]**: [Description]
- **[Feature 2]**: [Description]
- **[Feature 3]**: [Description]

---

## 🎯 **Key Success Metrics**

### **Technical Metrics**
- **Performance**: [Your performance targets]
- **Uptime**: [Your uptime targets]
- **Test Coverage**: [Your coverage targets]
- **Security**: [Your security targets]

### **Business Metrics**
- **[Metric 1]**: [Target]
- **[Metric 2]**: [Target]
- **[Metric 3]**: [Target]

---

## 🔄 **Documentation Update Workflow**

### **High Frequency Updates (Daily/Weekly)**
- **`docs/status/progress.yaml`** - Project progress and status changes

### **Medium Frequency Updates (Monthly)**
- **`docs/architecture/`** - System architecture changes
- **`docs/guides/`** - Process and setup changes

### **Low Frequency Updates (When Standards Change)**
- **`standards/`** - Development standards and patterns
- **`.ai/context.yaml`** - Project metadata changes

---

## 📚 **External Resources**

### **Official Documentation**
- **MCP Documentation**: https://modelcontextprotocol.io
- **Next.js Documentation**: https://nextjs.org/docs
- **[Your Backend] Documentation**: [URL]
- **[Your API] Documentation**: [URL]

### **Internal Standards**
- **Coding Standards**: `standards/coding.md`
- **Architectural Patterns**: `standards/patterns.md`
- **Business Rules**: `standards/business-rules.md`

---

## ✅ **Documentation Status**

- **Last Updated**: [Current Date]
- **Architecture Status**: ✅ Complete - AI-friendly structure established
- **MCP Integration**: ✅ Template ready for implementation
- **Quality**: ✅ High - Consistent, practical, and maintainable
- **Next Review**: Monthly or when major changes occur

---

## 🎯 **Next Steps**

1. **Customize This Template** - Update with your project specifics
2. **Set Up MCP Integration** - Follow MCP guide for AI assistance
3. **Review Current Status** - Check `docs/status/progress.yaml` for priorities
4. **Begin Development** - Start with your first priority
5. **Maintain Documentation** - Update status documents as progress is made

---

**This documentation follows AI-friendly architecture principles with single source of truth, machine-readable data, and clear hierarchy. Customize it to match your specific needs and use it to maintain consistency across all documentation files.**

**Remember**: Good documentation is like good code - it should be DRY (Don't Repeat Yourself), maintainable, and always up-to-date.

