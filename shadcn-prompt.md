now i want to enchace the use of shadcn

https://ui.shadcn.com/docs

https://ui.shadcn.com/docs/components

---

title: MCP Server

description: Use the shadcn MCP server to browse, search, and install components from registries.

---

The shadcn MCP Server allows AI assistants to interact with items from registries. You can browse available components, search for specific ones, and install them directly into your project using natural language.

For example, you can ask an AI assistant to "Build a landing page using components from the acme registry" or "Find me a login form from the shadcn registry".

Registries are configured in your project's `components.json` file.

```json title="components.json" showLineNumbers
{
  "registries": {
    "@acme": "https://acme.com/r/{name}.json"
  }
}
```

---

## Quick Start

**Run the following command** in your project:

```bash

npx shadcn@latest mcp init --client cursor

```

Open **Cursor Settings** and **Enable the MCP server** for shadcn. Then try the following prompts:

- Show me all available components in the shadcn registry

- Add the button, dialog and card components to my project

- Create a contact form using components from the shadcn registry

---

## What is MCP?

[Model Context Protocol (MCP)](https://modelcontextprotocol.io) is an open protocol that enables AI assistants to securely connect to external data sources and tools. With the shadcn MCP server, your AI assistant gains direct access to:

- **Browse Components** - List all available components, blocks, and templates from any configured registry

- **Search Across Registries** - Find specific components by name or functionality across multiple sources

- **Install with Natural Language** - Add components using simple conversational prompts like "add a login form"

- **Support for Multiple Registries** - Access public registries, private company libraries, and third-party sources

---

## How It Works

The MCP server acts as a bridge between your AI assistant, component registries and the shadcn CLI.

1. **Registry Connection** - MCP connects to configured registries (shadcn/ui, private registries, third-party sources)

2. **Natural Language** - You describe what you need in plain English

3. **AI Processing** - The assistant translates your request into registry commands

4. **Component Delivery** - Resources are fetched and installed in your project

---

## Supported Registries

The shadcn MCP server works out of the box with any shadcn-compatible registry.

- **shadcn/ui Registry** - The default registry with all shadcn/ui components

- **Third-Party Registries** - Any registry following the shadcn registry specification

- **Private Registries** - Your company's internal component libraries

- **Namespaced Registries** - Multiple registries configured with `@namespace` syntax

---

## Configuration

You can use any MCP client to interact with the shadcn MCP server. Here are the instructions for the most popular ones.

### Claude Code

To use the shadcn MCP server with Claude Code, add the following configuration to your project's `.mcp.json` file:

```json title=".mcp.json" showLineNumbers
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",

      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

After adding the configuration, restart Claude Code and run `/mcp` to see the shadcn MCP server in the list. If you see `Connected`, you're good to go.

See the [Claude Code MCP documentation](https://docs.anthropic.com/en/docs/claude-code/mcp) for more details.

### Cursor

To configure MCP in Cursor, add the shadcn server to your project's `.cursor/mcp.json` configuration file:

```json title=".cursor/mcp.json" showLineNumbers
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",

      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

After adding the configuration, enable the shadcn MCP server in Cursor Settings.

Once enabled, you should see a green dot next to the shadcn server in the MCP server list and a list of available tools.

See the [Cursor MCP documentation](https://docs.cursor.com/en/context/mcp#using-mcp-json) for more details.

### VS Code

To configure MCP in VS Code with GitHub Copilot, add the shadcn server to your project's `.vscode/mcp.json` configuration file:

```json title=".vscode/mcp.json" showLineNumbers
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",

      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

After adding the configuration, open `.vscode/mcp.json` and click **Start** next to the shadcn server.

See the [VS Code MCP documentation](https://code.visualstudio.com/docs/copilot/chat/mcp-servers) for more details.

---

## Configuring Registries

The MCP server supports multiple registries through your project's `components.json` configuration. This allows you to access components from various sources including private registries and third-party providers.

Configure additional registries in your `components.json`:

```json title="components.json" showLineNumbers
{
  "registries": {
    "@acme": "https://registry.acme.com/{name}.json",

    "@internal": {
      "url": "https://internal.company.com/{name}.json",

      "headers": {
        "Authorization": "Bearer ${REGISTRY_TOKEN}"
      }
    }
  }
}
```

<Callout>

**Note:** No configuration is needed to access the standard shadcn/ui

registry.

</Callout>

---

## Authentication

For private registries requiring authentication, set environment variables in your `.env.local`:

```bash title=".env.local"

REGISTRY_TOKEN=your_token_here

API_KEY=your_api_key_here

```

For more details on registry authentication, see the [Authentication documentation](/docs/registry/authentication).

---

## Example Prompts

Once the MCP server is configured, you can use natural language to interact with registries. Try one of the following prompts:

### Browse & Search

- Show me all available components in the shadcn registry

- Find me a login form from the shadcn registry

### Install Items

- Add the button component to my project

- Create a login form using shadcn components

- Install the Cursor rules from the acme registry

### Work with Namespaces

- Show me components from acme registry

- Install @internal/auth-form

- Build me a landing page using hero, features and testimonials sections from the acme registry

---

## Troubleshooting

### MCP Not Responding

If the MCP server isn't responding to prompts:

1. **Check Configuration** - Verify the MCP server is properly configured and enabled in your MCP client

2. **Restart MCP Client** - Restart your MCP client after configuration changes

3. **Verify Installation** - Ensure `shadcn` is installed in your project

4. **Check Network** - Confirm you can access the configured registries

### Registry Access Issues

If components aren't loading from registries:

1. **Check components.json** - Verify registry URLs are correct

2. **Test Authentication** - Ensure environment variables are set for private registries

3. **Verify Registry** - Confirm the registry is online and accessible

4. **Check Namespace** - Ensure namespace syntax is correct (`@namespace/component`)

### Installation Failures

If components fail to install:

1. **Check Project Setup** - Ensure you have a valid `components.json` file

2. **Verify Paths** - Confirm the target directories exist

3. **Check Permissions** - Ensure write permissions for component directories

4. **Review Dependencies** - Check that required dependencies are installed

### No Tools or Prompts

If you see the `No tools or prompts` message, try the following:

1. **Clear the npx cache** - Run `npx clear-npx-cache`

2. **Re-enable the MCP server** - Try to re-enable the MCP server in your MCP client

3. **Check Logs** - In Cursor, you can see the logs under View -> Output and select `MCP: project-*` in the dropdown.

---

## Learn More

- [Registry Documentation](/docs/registry) - Complete guide to shadcn registries

- [Namespaces](/docs/registry/namespace) - Configure multiple registry sources

- [Authentication](/docs/registry/authentication) - Secure your private registries

- [MCP Specification](https://modelcontextprotocol.io) - Learn about Model Context Protocol

i have this UI spec file

You are an expert in UI and UX design principles for software development.

Visual Design

- Establish a clear visual hierarchy to guide user attention.

- Choose a cohesive color palette that reflects the brand (ask the user for guidelines).

- Use typography effectively for readability and emphasis.

- Maintain sufficient contrast for legibility (WCAG 2.1 AA standard).

- Design with a consistent style across the application.

Interaction Design

- Create intuitive navigation patterns.

- Use familiar UI components to reduce cognitive load.

- Provide clear calls-to-action to guide user behavior.

- Implement responsive design for cross-device compatibility.

- Use animations judiciously to enhance user experience.

Accessibility

- Follow WCAG guidelines for web accessibility.

- Use semantic HTML to enhance screen reader compatibility.

- Provide alternative text for images and non-text content.

- Ensure keyboard navigability for all interactive elements.

- Test with various assistive technologies.

Performance Optimization

- Optimize images and assets to minimize load times.

- Implement lazy loading for non-critical resources.

- Use code splitting to improve initial load performance.

- Monitor and optimize Core Web Vitals (LCP, FID, CLS).

User Feedback

- Incorporate clear feedback mechanisms for user actions.

- Use loading indicators for asynchronous operations.

- Provide clear error messages and recovery options.

- Implement analytics to track user behavior and pain points.

Information Architecture

- Organize content logically to facilitate easy access.

- Use clear labeling and categorization for navigation.

- Implement effective search functionality.

- Create a sitemap to visualize overall structure.

Mobile-First Design

- Design for mobile devices first, then scale up.

- Use touch-friendly interface elements.

- Implement gestures for common actions (swipe, pinch-to-zoom).

- Consider thumb zones for important interactive elements.

Consistency

- Develop and adhere to a design system.

- Use consistent terminology throughout the interface.

- Maintain consistent positioning of recurring elements.

- Ensure visual consistency across different sections.

Testing and Iteration

- Conduct A/B testing for critical design decisions.

- Use heatmaps and session recordings to analyze user behavior.

- Regularly gather and incorporate user feedback.

- Continuously iterate on designs based on data and feedback.

Documentation

- Maintain a comprehensive style guide.

- Document design patterns and component usage.

- Create user flow diagrams for complex interactions.

- Keep design assets organized and accessible to the team.

Fluid Layouts

- Use relative units (%, em, rem) instead of fixed pixels.

- Implement CSS Grid and Flexbox for flexible layouts.

- Design with a mobile-first approach, then scale up.

Media Queries

- Use breakpoints to adjust layouts for different screen sizes.

- Focus on content needs rather than specific devices.

- Test designs across a range of devices and orientations.

Images and Media

- Use responsive images with srcset and sizes attributes.

- Implement lazy loading for images and videos.

- Use CSS to make embedded media (like iframes) responsive.

Typography

- Use relative units (em, rem) for font sizes.

- Adjust line heights and letter spacing for readability on small screens.

- Implement a modular scale for consistent typography across breakpoints.

Touch Targets

- Ensure interactive elements are large enough for touch (min 44x44 pixels).

- Provide adequate spacing between touch targets.

- Consider hover states for desktop and focus states for touch/keyboard.

Performance

- Optimize assets for faster loading on mobile networks.

- Use CSS animations instead of JavaScript when possible.

- Implement critical CSS for above-the-fold content.

Content Prioritization

- Prioritize content display for mobile views.

- Use progressive disclosure to reveal content as needed.

- Implement off-canvas patterns for secondary content on small screens.

Navigation

- Design mobile-friendly navigation patterns (e.g., hamburger menu).

- Ensure navigation is accessible via keyboard and screen readers.

- Consider using a sticky header for easy navigation access.

Forms

- Design form layouts that adapt to different screen sizes.

- Use appropriate input types for better mobile experiences.

- Implement inline validation and clear error messaging.

Testing

- Use browser developer tools to test responsiveness.

- Test on actual devices, not just emulators.

- Conduct usability testing across different device types.

Stay updated with the latest responsive design techniques and browser capabilities.

Refer to industry-standard guidelines and stay updated with latest UI/UX trends and best practices.

---
