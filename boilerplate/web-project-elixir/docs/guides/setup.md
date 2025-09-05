# Setup Guide - Phoenix Web Boilerplate

## Prerequisites
- Erlang/OTP 25+
- Elixir 1.14+
- PostgreSQL

## Install
Follow official Elixir install: https://elixir-lang.org/docs.html

## Create Project
```bash
mix archive.install hex phx_new
mix phx.new my_app --live
cd my_app
mix ecto.create
mix phx.server
```

## Notes
- LiveView included via --live
- Tailwind configured by default in Phoenix 1.7+
- Configure DATABASE_URL and secrets for prod

## References
- Elixir Docs: https://elixir-lang.org/docs.html
- Phoenix Overview: https://hexdocs.pm/phoenix/overview.html
