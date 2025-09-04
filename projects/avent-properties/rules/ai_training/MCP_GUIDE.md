# 📘 MCP Integration Guide - Avent Properties

**Purpose:** Single, authoritative guide for MCP integration in Avent Properties - consolidating all MCP-related documentation into one practical resource.

**Target Audience:** AI assistants, developers, technical leads

**Approach:** Practical integration with current project status, eliminating complexity and focusing on real value

**Current Status:** No MCP implementation yet - this guide provides the path forward

---

## 🎯 **What This Guide Consolidates**

This document replaces and consolidates:
- ❌ `MCP_INTEGRATION_GUIDE.md` (536 lines - overly complex)
- ❌ `gh_mcp_cursor_integration.md` (401 lines - redundant)
- ❌ `MCP_GUIDE_with_repo-data.md` (290 lines - GitHub integration details)
- ✅ **This single guide** - practical, current, and actionable

---

## 🧩 **MCP Basics for Avent Properties**

### **What is MCP?**
- **MCP (Model Context Protocol)**: Protocol for AI agents to access project context
- **Purpose**: Give AI assistants access to your documentation, business rules, and codebase
- **Benefit**: AI agents understand your project instead of guessing

### **Why We Need It**
- **Current Problem**: AI agents don't know your project structure
- **Solution**: MCP provides access to `/rules` and `/docs` folders
- **Result**: Better code generation, fewer errors, faster development

---

## ⚙️ **Simple MCP Setup (2 Servers - Current Implementation)**

### **1. Current Cursor Configuration**

Your `.cursor/mcp.json` is already configured with a 2-server setup:

```json
{
  "mcpServers": {
    "avent-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "avent-rules": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./rules"]
    }
  }
}
```

### **2. Install Required Package**

```bash
# Install MCP filesystem server
yarn add -D @modelcontextprotocol/server-filesystem

# Verify installation
npx @modelcontextprotocol/server-filesystem --help
```

### **3. What This Gives You**

- **`avent-docs`**: Access to project status, implementation details, migration guides
- **`avent-rules`**: Access to business logic, development standards, coding rules

---

## 🚀 **Enhanced Setup (Optional - Phase 2)**

### **GitHub Integration (When Ready)**

```json
{
  "mcpServers": {
    "avent-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "avent-rules": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./rules"]
    },
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  },
  "inputs": [
    {
      "type": "promptString",
      "id": "github_token",
      "description": "GitHub Personal Access Token",
      "password": true
    }
  ]
}
```

**Benefits:**
- Repository access and workflow monitoring
- Issue and PR management
- CI/CD integration and security monitoring

---

## 🔧 **GitHub MCP Server Setup (Advanced)**

### **Prerequisites**
- **Docker Desktop**: Must be installed and running
- **GitHub PAT**: Personal Access Token with appropriate scopes
- **Cursor version**: v0.48.0+ for Streamable HTTP support

### **GitHub Token Permissions**
```
✅ repo (Full control of private repositories)
✅ read:packages (Download packages from GitHub Package Registry)
✅ read:org (Read org and team data)
✅ workflow (Update GitHub Action workflows)
```

### **Security Best Practices**
- **Never commit tokens** to version control
- **Use minimum required permissions** for your use case
- **Rotate tokens regularly** (every 90 days)
- **Store tokens securely** in environment variables

---

## 📚 **What AI Agents Can Access**

### **Through `avent-docs` Server**
- Project implementation status
- Database setup and migration guides
- Deployment configuration
- Current development priorities

### **Through `avent-rules` Server**
- Business logic and requirements
- Development methodology
- Coding standards and patterns
- Frontend development guides

### **Through `github` Server (When Added)**
- Repository structure and code analysis
- Issue and PR management
- CI/CD workflow monitoring
- Security alerts and Dependabot updates

---

## 🎯 **Practical Usage Examples**

### **Component Generation**
```
"Generate a PropertyCard component using our glassmorphism design system"
```

**MCP Context:** AI agent accesses design system patterns from `/rules/frontend/`

### **GraphQL Integration**
```
"Create a GraphQL hook for property filtering based on our Supabase architecture"
```

**MCP Context:** AI agent accesses GraphQL migration guide from `/docs/`

### **Business Logic**
```
"Implement the 10% deposit rule for tour reservations"
```

**MCP Context:** AI agent accesses business requirements from `/rules/DEVELOPMENT_BLUEPRINT.md`

### **GitHub Integration (When Available)**
```
"Analyze our GitHub repository structure and suggest improvements"
```

**MCP Context:** AI agent accesses repository information and workflow status

---

## 🚀 **MCP-Use Integration (Future Enhancement)**

### **Custom Agent Development**
- Connect any LLM to any MCP server
- Build custom agents for specific workflows
- Multi-server support for complex tasks

### **Use Cases for Avent Properties**
- Development automation and code reviews
- Documentation management and updates
- Code quality analysis and security monitoring
- Workflow integration and team collaboration

---

## 🧪 **Testing Your MCP Setup**

### **1. Verify MCP Servers**
```bash
# Test each MCP server
npx @modelcontextprotocol/server-filesystem ./docs --help
npx @modelcontextprotocol/server-filesystem ./rules --help
```

### **2. Test in Cursor AI**
1. Restart Cursor AI after creating `cursor/config.json`
2. Ask AI agent: "What's our current project status?"
3. AI should reference `/docs/IMPLEMENTATION_TRACKER.md`

### **3. Test Context Access**
```
"Show me our current development priorities"
```
AI should access and summarize from implementation tracker.

### **4. Test GitHub Integration (When Available)**
```
"Can you access our GitHub repository and show me the current status?"
```

---

## 🚫 **What We Don't Need (Avoiding Overkill)**

- ❌ **Complex MCP Architecture**: We don't need 6+ servers
- ❌ **Database MCP Server**: Existing GraphQL hooks work fine
- ❌ **Custom MCP Servers**: Filesystem access is sufficient
- ❌ **Enterprise Features**: Keep it simple and practical

---

## 📈 **Success Metrics**

### **Phase 1 Success (Immediate)**
- [ ] MCP servers operational and accessible
- [ ] AI agents can reference project documentation
- [ ] Basic component generation follows patterns
- [ ] Reduced AI generation errors

### **Phase 2 Success (Future)**
- [ ] GitHub integration working
- [ ] Enhanced workflow automation
- [ ] Improved development efficiency
- [ ] Better AI agent understanding

---

## 🔧 **Troubleshooting**

### **Common Issues**

#### **MCP Servers Not Found**
```bash
# Check if package is installed
yarn list @modelcontextprotocol/server-filesystem

# Reinstall if needed
yarn add -D @modelcontextprotocol/server-filesystem
```

#### **Cursor AI Not Recognizing MCP**
1. Ensure `cursor/config.json` is in project root
2. Restart Cursor AI completely
3. Check Cursor AI logs for MCP errors

#### **Permission Issues**
```bash
# Make sure MCP servers are executable
chmod +x node_modules/.bin/@modelcontextprotocol/server-filesystem
```

#### **GitHub MCP Server Issues**
- **Docker not running**: Ensure Docker Desktop is started
- **Image pull failures**: Try `docker logout ghcr.io` then retry
- **Permission errors**: Check Docker Desktop permissions
- **Invalid token**: Verify PAT has correct scopes and hasn't expired

---

## 🎯 **Next Steps**

### **Immediate Actions**
1. **Create `cursor/config.json`** with 2-server setup
2. **Install MCP filesystem server** package
3. **Test basic MCP integration** in Cursor AI
4. **Verify AI agents can access** project documentation

### **Future Enhancements**
1. **Add GitHub MCP server** when ready for workflow automation
2. **Evaluate MCP-Use library** for custom agent development
3. **Consider Fast API MCP** for business feature automation

---

## 📚 **Resources**

- **Official MCP Documentation**: https://modelcontextprotocol.io
- **MCP Server Development**: https://modelcontextprotocol.io/docs/server-development
- **GitHub MCP Server**: https://github.com/github/github-mcp-server
- **Project Documentation**: `/docs` and `/rules` folders (accessible via MCP)

---

## ✅ **Conclusion**

This consolidated guide provides everything you need for practical MCP integration:

1. **Simple 2-server setup** for immediate value
2. **Clear implementation steps** without complexity
3. **Practical usage examples** for real development
4. **Future enhancement path** when ready
5. **GitHub integration** for advanced workflow automation
6. **MCP-Use capabilities** for custom agent development

**Start with the basic setup, test it works, then enhance gradually. Keep it simple and practical.**

---

## 🔄 **Documentation Status**

- **Last Updated**: January 2025
- **Consolidated From**: 4 separate MCP guides (1,200+ lines)
- **Current Status**: Single source of truth for MCP integration
- **Next Review**: When MCP configuration changes or new features added