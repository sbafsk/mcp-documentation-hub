# AI Agent Instructions

## Context Loading Priority

1. **Primary Context**: `.ai/context.yaml` (always load first)
2. **Current Status**: `docs/status/progress.yaml` (for status queries)
3. **Implementation Details**: Specific files as needed
4. **Standards**: `standards/` directory (for code generation)

## Query Routing

- **Current status**: `docs/status/progress.yaml`
- **Project overview**: `docs/index.md`
- **How to setup**: `docs/guides/setup.md`
- **Coding standards**: `standards/coding.md`
- **Architecture details**: `docs/architecture/overview.md`

## AI Behavior Guidelines

- Always check `.ai/context.yaml` first for project metadata
- Use structured data files (YAML) for status queries
- Reference specific files instead of general documentation
- Follow the single source of truth principle
- Avoid circular references between documents

## Context Injection Pattern

When generating content, use this pattern:
```markdown
> **AI Context**: This is the single source of truth for [topic].
> For current status: see `docs/status/progress.yaml`
> For implementation details: see `docs/architecture/overview.md`
```