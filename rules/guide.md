# 📘 Training Guide – Using MCP in Cursor AI for Avent Properties

This guide will help train your **AI Development Agent in Cursor AI** to work with the **Model Context Protocol (MCP)** in a practical way, applied to your daily workflow with **Next.js, GraphQL, and Supabase**.

---

## 🎯 Training Objectives

1. Understand what MCP is and how it improves development for Avent Properties.
2. Learn how to configure `cursor/config.json` for MCP with our simplified approach.
3. Access project documentation and business rules through MCP servers.
4. Create efficient workflows with MCP inside Cursor AI.
5. Maintain a reproducible standard for your team.

---

## 🧩 Key MCP Concepts

- **MCP (Model Context Protocol)**: A protocol that defines how agents (e.g., Cursor AI) interact with different **context sources** (code, documentation, business rules).
- **MCP Server**: A data source or tool that exposes resources to Cursor (e.g., project docs, business rules, implementation guides).
- **Config.json**: The central file where you define which MCP servers are available.
- **Context Providers**: Connectors between Cursor and your resources (e.g., documentation, business logic).
- **Enriched Prompts**: The agent uses MCP context to provide more accurate and relevant answers.

---

## ⚙️ Initial Setup for Avent Properties

Inside your project, create the following file:

```jsonc
// cursor/config.json
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

🔹 This setup enables:

- **avent-docs**: Access to project status, implementation details, and technical guides
- **avent-rules**: Access to business logic, development standards, and AI training materials

---

## 🚀 Suggested Workflow

### **Define MCP Servers**
- Add your main resources in config.json (docs + rules folders)
- Keep it simple - we don't need complex database or API servers

### **Train the Agent**
- Explain your stack (Next.js, GraphQL, Supabase, React)
- Provide access to your project documentation via MCP
- Focus on practical development patterns, not enterprise complexity

### **Use Dynamic Contexts**
Example prompts:
- "Generate a PropertyCard component using our glassmorphism design system"
- "Create a GraphQL hook for property filtering based on our Supabase architecture"
- "Implement advanced React patterns using our completed patterns guide"

### **Automatic Validation**
- The agent can validate components against your design system
- Check GraphQL operations against your hybrid architecture
- Ensure consistency with established patterns

---

## 📌 Best Practices for Avent Properties

- **Keep config.json simple** - We use 2 servers, not 6
- **Focus on practical value** - Documentation access, not complex integrations
- **Use .env for sensitive data** - Never hardcode secrets
- **Document in the repo** - How to install and run MCP servers
- **Use npx in commands** - Ensures consistent versions
- **Sync cursor/config.json** - Across your team

---

## 🏋️ Training Exercises

### **Level 1 – Basic**
- Configure avent-docs and avent-rules servers
- Ask Cursor to summarize a component file
- Example prompt: "Explain this PropertyCard component in 3 bullet points"

### **Level 2 – Intermediate**
- Generate components using our design system
- Create GraphQL hooks based on our Supabase architecture
- Example prompt: "Create a usePropertyFilters hook for our hybrid GraphQL setup"

### **Level 3 – Advanced**
- Implement advanced React patterns from our guides
- Build complex workflows using our business logic
- Example prompt: "Implement the State Reducer pattern for our property management system"

---

## 🎯 What We Don't Need (Overkill)

- ❌ Complex MCP server architecture (6 servers)
- ❌ Database server integration (existing GraphQL hooks work great)
- ❌ Audit logging and compliance systems
- ❌ Enterprise-level security layers

---

## ✅ Conclusion

With MCP, your AI Agent in Cursor evolves from a generic copilot into a true team member, connected to your sources of truth: project documentation, business rules, and implementation guides.

This means less friction, more context, and faster development while maintaining the simplicity and practicality that makes our approach effective.