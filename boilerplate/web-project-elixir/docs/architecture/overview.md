# System Architecture Overview - Phoenix Web

> AI Context: Single source of truth for system architecture.
> Status: `../status/progress.yaml` · Priorities: `../status/priorities.md`
> Official docs: https://elixir-lang.org/docs.html · https://hexdocs.pm/phoenix/overview.html

## Layers
- Presentation: Phoenix Controllers/LiveView/Components
- Domain: Contexts with pure functions and business rules
- Data: Ecto Schemas/Changesets, Repos, Queries

## Key Decisions
- Context-first design per Phoenix guides
- LiveView for real-time UI
- Ecto with Postgres; query composition & preload
- ExUnit for tests; Mox for behaviours when needed

## Project Structure (typical)
```
lib/
  my_app/
    web/            # controllers, live, components
    accounts/       # context example
    core/           # domain modules
  my_app.ex
priv/repo/
  migrations/
  seeds.exs
```

## Security & Performance
- CSRF protection, parameters validation via changesets
- Indexing, avoid N+1 with preload, caching via ETS/Redis (optional)

## References
- Elixir Docs: https://elixir-lang.org/docs.html
- Phoenix Overview: https://hexdocs.pm/phoenix/overview.html
