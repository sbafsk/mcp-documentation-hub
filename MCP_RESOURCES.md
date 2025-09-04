# MCP Resources & Integration Guides

> **AI Context**: This is the single source of truth for MCP (Model Context Protocol) resources and integration guides.
> For project documentation architecture: see `DOCUMENTATION_ARCHITECTURE.md`
> For project setup: see `SETUP.md`

## Overview

This document consolidates all MCP-related resources, integration guides, and tools for the MCP Documentation Hub. It serves as the central reference for MCP implementation across all projects.

## Core MCP Concepts

### What is MCP?
Model Context Protocol (MCP) is an open protocol that enables AI assistants to securely connect to external data sources and tools. It allows AI agents to:
- Access project documentation and context
- Interact with development tools
- Execute commands and workflows
- Maintain consistent development assistance

### Key Benefits
- **Context Injection**: AI agents get immediate access to project context
- **Tool Integration**: Connect AI to development tools and workflows
- **Consistent Assistance**: Standardized AI development support across projects
- **Security**: Secure, controlled access to project resources

## MCP Server Setup

### 1. GitHub MCP Server

The GitHub MCP Server connects AI tools directly to GitHub's platform, enabling:
- Repository management and code browsing
- Issue and PR automation
- CI/CD workflow intelligence
- Code analysis and security insights

#### Installation Options

**Remote Server (Recommended)**
```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  }
}
```

**Local Server (Docker)**
```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_PAT"
      }
    }
  }
}
```

#### Prerequisites
- GitHub Personal Access Token with appropriate scopes
- Cursor IDE v0.48.0+ for remote server
- Docker for local server setup

### 2. shadcn/ui MCP Server

The shadcn MCP Server allows AI assistants to browse, search, and install components from registries.

#### Quick Setup
```bash
npx shadcn@latest mcp init --client cursor
```

#### Configuration
```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

#### Example Prompts
- "Show me all available components in the shadcn registry"
- "Add the button, dialog and card components to my project"
- "Create a contact form using components from the shadcn registry"

### 3. MCP Use Library

A Python library that connects any LLM to any MCP server without requiring a separate MCP client.

#### Installation
```bash
pip install mcp-use
```

#### Basic Usage
```python
import asyncio
from langchain_openai import ChatOpenAI
from mcp_use import MCPAgent, MCPClient

async def main():
    config = {
        "mcpServers": {
            "playwright": {
                "command": "npx",
                "args": ["@playwright/mcp@latest"]
            }
        }
    }
    
    client = MCPClient.from_dict(config)
    llm = ChatOpenAI(model="gpt-4o")
    agent = MCPAgent(llm=llm, client=client, max_steps=30)
    
    result = await agent.run("Find the best restaurant in San Francisco")
    print(result)

asyncio.run(main())
```

## Integration Guides

### Cursor IDE Integration

1. **Create Configuration**: Add `.cursor/mcp.json` to your project root
2. **Configure Servers**: Add MCP server configurations
3. **Enable in Settings**: Enable MCP servers in Cursor Settings
4. **Test Integration**: Use natural language prompts to test functionality

### VS Code Integration

1. **Install GitHub Copilot**: Ensure you have GitHub Copilot installed
2. **Create Configuration**: Add `.vscode/mcp.json` to your project
3. **Start Server**: Click "Start" next to configured servers
4. **Use Agent Mode**: Toggle Agent mode in Copilot Chat

### Claude Desktop Integration

1. **Global Configuration**: Add to `~/.claude/mcp.json`
2. **Project Configuration**: Add to project-specific `.mcp.json`
3. **Restart Claude**: Restart Claude Desktop after configuration
4. **Verify Connection**: Check for green dot in MCP server list

## Project-Specific MCP Setup

### Documentation Server
```json
{
  "mcpServers": {
    "project-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    }
  }
}
```

### Standards Server
```json
{
  "mcpServers": {
    "project-standards": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./standards"]
    }
  }
}
```

### Multi-Server Configuration
```json
{
  "mcpServers": {
    "project-docs": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./docs"]
    },
    "project-standards": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "./standards"]
    },
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  }
}
```

## Best Practices

### Security
- Use environment variables for sensitive tokens
- Implement proper access controls
- Regularly rotate API keys and tokens
- Use read-only mode when appropriate

### Performance
- Enable only necessary toolsets
- Use dynamic tool discovery for large tool sets
- Implement proper error handling
- Monitor resource usage

### Development Workflow
- Test MCP integration in development first
- Document MCP configurations in project README
- Use consistent naming conventions
- Maintain separate configurations for different environments

## Troubleshooting

### Common Issues

**MCP Server Not Responding**
- Check configuration syntax
- Restart MCP client
- Verify server installation
- Check network connectivity

**Authentication Failures**
- Verify API tokens and keys
- Check token permissions and scopes
- Ensure environment variables are set
- Test authentication independently

**Tool Access Issues**
- Verify tool permissions
- Check server configuration
- Review error logs
- Test with minimal configuration

### Debug Mode
Enable debug mode for detailed logging:
```bash
DEBUG=2 python your_script.py
```

## Related Resources

### Official Documentation
- [Model Context Protocol](https://modelcontextprotocol.io)
- [GitHub MCP Server](https://github.com/github/github-mcp-server)
- [shadcn/ui MCP](https://ui.shadcn.com/docs)
- [MCP Use Library](https://github.com/pietrozullo/mcp-use)

### Project Documentation
- [Documentation Architecture](DOCUMENTATION_ARCHITECTURE.md)
- [Project Setup Guide](SETUP.md)
- [Boilerplate Documentation](boilerplate/)

### Community Resources
- [MCP Discord Community](https://discord.gg/XkNkSkMz3V)
- [Awesome MCP Tools](https://github.com/pietrozullo/mcp-use)
- [MCP Examples](https://github.com/mcp-use/mcp-use)

## Contributing

To contribute to MCP resources:
1. Follow the documentation architecture principles
2. Test configurations across different environments
3. Update this document when adding new resources
4. Maintain backward compatibility
5. Document any breaking changes

---

**This document serves as the central hub for all MCP-related resources and should be updated whenever new MCP tools or configurations are added to the project.**
