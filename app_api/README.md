# Litestar API Backend

Async Python API backend built with Litestar, SQLAlchemy, and PostgreSQL. Features JWT authentication, role-based permissions, and a modular architecture.

## Requirements
- [uv](https://docs.astral.sh/uv/)
- [just](https://github.com/casey/just)

## Quick Start

```bash
uv sync                    # Install dependencies
cp config{.example,}.toml  # Copy config (edit if necessary)
just run                   # Run development server
```

## Development Commands

```bash
# Database migrations
just db upgrade --no-prompt  # Apply migrations

# Code quality
just format                  # Format code with ruff
just lint                    # Lint and auto-fix
just ty                      # Type checking with ty
just pyrefly                 # Type checking with pyrefly

# Testing
just test                    # Run all tests
just test tests/accounts/    # Run specific tests
uv run pytest -v --tb=no     # Custom pytest options
```

## Architecture

### Structure
- **app/main.py** - Application factory
- **app/config.py** - Settings management (uses `config.toml`)
- **app/db.py** - SQLAlchemy configuration
- **app/models/** - ORM models
- **app/api/** - Feature modules by domain (accounts, etc.)

### Authentication
- OAuth2 password bearer flow with JWT tokens
- Password hashing with Argon2

### Permissions
- Role-based access control via guards
- Guards: `has_permission()`, `has_role()`, `has_any_permission()`, `has_all_permissions()`
- Superuser role bypasses permission checks

### Database Models
- Typed SQLAlchemy 2.x model
- Advanced Alchemy base models with UUIDv7 primary keys

## Configuration

Edit `config.toml` or set environment variables:
- `database_url` - PostgreSQL connection URL
- `secret_key` - JWT secret key
- `cors_allowed_origins` - List of allowed CORS origins
- `superuser_role_name` - Role with all permissions

## Testing

Uses pytest with Docker PostgreSQL via `pytest-databases`:
- Session-scoped test database per worker
- Function-scoped `clean_database` fixture
- Parallel testing supported with `pytest-xdist`
