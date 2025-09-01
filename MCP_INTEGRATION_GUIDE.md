# Phase 1: MCP Integration Guide - Avent Properties

**Purpose:** Phase 1 AI agent training - MCP configuration and basic integration for Cursor AI agents.

**Target Audience:** Cursor AI agents (Phase 1 training)

**Training Phase:** Foundation setup and MCP integration

**Prerequisites:** None - this is the starting point for AI agent training

---

## 🎯 Phase 1 Objectives

**Goal:** Establish MCP integration and basic context understanding for AI agents working on Avent Properties.

**Success Criteria:**
- MCP servers configured and operational
- AI agent can access project documentation through MCP
- Basic component generation follows established patterns
- GraphQL integration works with hybrid architecture

**What AI Agent Will Learn:**
- Project structure and documentation access
- Basic business context (luxury real estate platform)
- Technical architecture (Next.js + Supabase + GraphQL)
- Design system patterns (glassmorphism + gold accents)
- Development standards and coding conventions

---

## 🏗️ MCP Configuration Setup

### **1. Create Cursor Configuration**

Create `cursor/config.json` in your project root:

```json
{
  "mcpServers": {
    "avent-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"],
      "env": {
        "DOC_TYPE": "project_status_and_implementation",
        "UPDATE_FREQUENCY": "daily_weekly"
      }
    },
    "avent-rules": {
      "command": "npx", 
      "args": ["@modelcontextprotocol/server-filesystem", "./rules"],
      "env": {
        "CONTEXT_TYPE": "business_logic_and_requirements",
        "PROJECT_NAME": "Avent Properties",
        "DOMAIN": "luxury_real_estate_uruguay"
      }
    }
  }
}
```

### **2. Install Required MCP Servers**

```bash
# Install official MCP server
yarn add -D @modelcontextprotocol/server-filesystem

# Verify installation
npx @modelcontextprotocol/server-filesystem --help
```

---

## 📚 MCP Context Understanding

### **Documentation Access Through MCP**

MCP servers provide AI agents with access to existing documentation:

#### **Business Context** → `rules/DEVELOPMENT_BLUEPRINT.md`
- **Target Market:** HNWIs in Dubai/UAE seeking luxury properties in Uruguay
- **Revenue Model:** 3% + VAT commission from both buyer and seller
- **Service:** Curated luxury real estate tours with 10% deposit requirement
- **Geographic Focus:** Punta del Este, José Ignacio, Piriápolis

#### **Technical Architecture** → `docs/ENDPOINT_MIGRATION_GUIDE.md`
- **Hybrid GraphQL:** 72% Supabase auto-generated, 28% custom Apollo Server
- **Performance:** 70% reduction in database queries with N+1 elimination
- **Custom Hooks:** useProperties(), useAgencies(), useReservations(), etc.
- **Migration Status:** Complete and production-ready

#### **Development Standards** → `rules/RULES.md`
- **Code Style:** Tabs, single quotes, no semicolons, strict equality
- **Naming:** PascalCase components, camelCase variables, kebab-case files
- **Architecture:** Functional components, TypeScript strict mode, GraphQL only
- **Testing:** Jest + React Testing Library, Playwright E2E

#### **Current Status** → `docs/IMPLEMENTATION_TRACKER.md`
- **MVP Features:** 100% complete (8/8) ✅
- **GraphQL Migration:** 100% complete ✅
- **Advanced React Patterns:** 100% complete (6/6 patterns) ✅
- **Test Coverage:** 138/138 tests passing with zero warnings ✅
- **Current Phase:** Production Deployment & Advanced Features
- **Next Focus:** Stripe integration, email notifications, admin dashboard

---

## 🎨 Component Development with MCP

### **Reference Design System Documentation**

For component development patterns, refer to:
- `rules/frontend/AI_AGENT_MASTER_GUIDE.md` - Component architecture and design system
- `rules/frontend/IMPLEMENTATION_MASTER_GUIDE.md` - Complete implementation examples
- `rules/frontend/ADVANCED_REACT_PATTERNS.md` - Advanced React patterns guide

### **MCP-Enhanced Development**

With MCP integration, AI agents will have access to:
- Existing component patterns and examples
- Design system tokens and styling guidelines
- Advanced React patterns implementation
- GraphQL integration patterns

---

## 🔌 GraphQL Integration with MCP

### **Reference GraphQL Documentation**

For complete GraphQL architecture details, refer to:
- `docs/ENDPOINT_MIGRATION_GUIDE.md` - Hybrid architecture implementation (100% complete)
- `lib/graphql/resolvers-migration-plan.ts` - Migration status and decisions
- `lib/hooks/index.ts` - Available custom React Query hooks
- `lib/types/graphql.ts` - TypeScript type definitions

### **MCP-Enhanced GraphQL Development**

With MCP integration, AI agents will have access to:
- Live database schema and relationships
- Current migration status (100% complete - 72% Supabase, 28% Apollo)
- Available custom hooks and their usage patterns
- Performance optimization guidelines

---

## 🛠️ Development Workflow with MCP

### **Reference Development Standards**

For complete development guidelines, refer to:
- `rules/RULES.md` - Coding standards and best practices
- `rules/DEVELOPMENT_WORKFLOW.md` - Development methodology
- `rules/frontend/AI_AGENT_MASTER_GUIDE.md` - Component development checklist

### **MCP-Enhanced Development Process**

With MCP integration, AI agents will:
- Access existing component patterns and examples
- Follow established coding standards automatically
- Use correct GraphQL hooks based on current architecture
- Apply proper design system tokens and styling
- Maintain consistency with existing codebase patterns

---

## 🎯 Phase 1 Training Prompts

### **Basic Component Generation**
```
"Generate a PropertyCard component using our glassmorphism design system with gold accents"
```

### **GraphQL Integration**
```
"Create a usePropertyFilters hook that works with our Supabase GraphQL architecture"
```

### **Advanced React Patterns (All 6 Complete)**
```
"Implement advanced React patterns using our completed Context Module Functions, Compound Components, State Reducer, Control Props, and Prop Collections patterns"
```

### **Business Logic Implementation**
```
"Generate a reservation form component that handles the 10% deposit requirement for luxury property tours"
```

### **Context Understanding Tests**
```
"What is the business model described in our documentation?"
"How does our hybrid GraphQL architecture work?"
"What design system patterns should I follow?"
```

---

## 🔍 Testing & Validation

### **MCP Integration Testing**

#### **1. Verify MCP Servers**
```bash
# Test each MCP server
npx @modelcontextprotocol/server-filesystem --help

# Check configuration
cat cursor/config.json
```

#### **2. Test Context Understanding**
Ask the AI agent to reference existing documentation:
- "What is the business model described in DEVELOPMENT_BLUEPRINT.md?"
- "How does the hybrid GraphQL architecture work according to ENDPOINT_MIGRATION_GUIDE.md?"
- "What design system patterns are defined in AI_AGENT_MASTER_GUIDE.md?"

#### **3. Test Code Generation**
Request components and verify they:
- Follow established patterns from existing documentation
- Use correct styling and colors from design system
- Integrate properly with the hybrid architecture
- Maintain performance optimizations

---

## 🚀 Advanced MCP Features

### **Custom MCP Server Development**

For project-specific needs, consider creating custom MCP servers that reference your existing documentation structure.

### **Integration with Development Tools**

MCP can be extended to work with VS Code extensions and other development tools for enhanced integration.

---

## 📊 Monitoring & Optimization

### **MCP Performance Metrics**

Track these metrics to ensure optimal MCP integration:
- Context loading time: < 2 seconds
- Code generation quality: > 95% pattern compliance
- Developer experience: > 90% relevance

---

## 🔧 Troubleshooting

### **Common Issues & Solutions**

#### **MCP Server Connection Issues**
```bash
# Check server status
ps aux | grep mcp

# Restart MCP servers
pkill -f mcp
# Restart Cursor AI
```

#### **Context Loading Problems**
- Verify file paths in config.json
- Check environment variables
- Ensure proper permissions

#### **Code Generation Issues**
- Verify design system tokens
- Check GraphQL hook availability
- Validate component patterns

---

## 📚 Additional Resources

### **MCP Documentation**
- [Official MCP Documentation](https://modelcontextprotocol.io)
- [MCP Server Development Guide](https://modelcontextprotocol.io/docs/server-development)
- [MCP Client Integration](https://modelcontextprotocol.io/docs/client-integration)

### **Project Documentation References**
- `rules/` - AI learning materials and business logic
- `docs/` - Project status and implementation details
- `rules/frontend/` - Frontend development guides and patterns

---

## 🎯 Success Criteria

### **MCP Integration Success**

The MCP integration is successful when:
- AI agents can access and understand existing documentation
- Code generation follows established patterns
- Development efficiency is improved
- Team collaboration is enhanced

---

## 🚀 Phase 1 Completion Checklist

### **Setup & Configuration**
- [ ] Create `cursor/config.json` with provided MCP configuration
- [ ] Install required MCP servers (`@modelcontextprotocol/server-filesystem`)
- [ ] Restart Cursor AI to load MCP configuration
- [ ] Verify MCP servers are operational

### **Basic Understanding Validation**
- [ ] AI agent can access and reference project documentation
- [ ] AI agent understands basic business context (luxury real estate, HNWI clients)
- [ ] AI agent knows technical architecture (Next.js + Supabase + GraphQL)
- [ ] AI agent follows design system patterns (glassmorphism + gold accents)

### **Basic Implementation Testing**
- [ ] Generate simple components following established patterns
- [ ] Create basic GraphQL hooks using Supabase architecture
- [ ] Apply design system tokens correctly
- [ ] Follow coding standards and conventions

### **Phase 1 Success Criteria**
- [ ] MCP integration working properly
- [ ] AI agent has basic project context understanding
- [ ] Component generation follows established patterns
- [ ] GraphQL integration works with hybrid architecture
- [ ] AI agent understands all 6 advanced React patterns are complete
- [ ] Ready to proceed to Phase 2 (BMAD Integration)

---

## 🔄 **Phase 1 → Phase 2 Transition**

**When Phase 1 is complete:**
1. AI agent has basic MCP integration and project understanding
2. Can generate components following established patterns
3. Understands business context and technical architecture
4. Ready for advanced training with BMAD methodology

**Next:** Proceed to `BMAD_INTEGRATION_GUIDE.md` for Phase 2 training
