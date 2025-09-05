# Deployment Guide - Phoenix Web Boilerplate

## Production Build
```bash
MIX_ENV=prod mix deps.get --only prod
MIX_ENV=prod mix compile
MIX_ENV=prod mix assets.deploy
MIX_ENV=prod mix release
_build/prod/rel/my_app/bin/my_app start
```

## Database
- Set DATABASE_URL
- Run migrations on boot: `bin/my_app eval "MyApp.Release.migrate"`

## References
- Elixir Docs: https://elixir-lang.org/docs.html
- Phoenix Overview: https://hexdocs.pm/phoenix/overview.html
