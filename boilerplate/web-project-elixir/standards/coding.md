# Coding Standards - Phoenix Web

## Language & Formatting
- Elixir 1.14+, Erlang/OTP 25+
- Format with `mix format`
- Follow Elixir style guide

## Project Structure
- Use Phoenix contexts to organize domains
- Keep controllers thin; business logic in contexts
- Use LiveView components for UI composition

## Database & Ecto
- Use changesets for validation
- Prefer query composition and preload to avoid N+1
- Add appropriate indexes

## Testing
- Use ExUnit; test contexts, controllers, and LiveViews
- Use data factories (e.g., ExMachina)

## Security
- CSRF protection enabled
- Validate params via changesets
- Use HTTPS in production

## References
- Elixir Docs: https://elixir-lang.org/docs.html
- Phoenix Overview: https://hexdocs.pm/phoenix/overview.html
