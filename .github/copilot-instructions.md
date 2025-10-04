# Dog Shelter Workshop Project

This is a workshop repository teaching GitHub Copilot and GitHub DevOps workflows through a fictional dog shelter website. The project uses a monorepo structure with separate client and server codebases.

## Architecture

**Monorepo structure:**
- `server/` - Flask REST API with SQLAlchemy ORM
- `client/` - Astro frontend with Svelte components
- `content/` - Workshop materials and documentation (DO NOT modify)
- `scripts/` - Development helper scripts for starting servers

**Data flow:** Client (Astro/Svelte) → REST API (`/api/*` routes) → Flask → SQLAlchemy → SQLite DB

## Backend (Flask + SQLAlchemy)

- **Language:** Python 3.x with type hints (always use)
- **Framework:** Flask with SQLAlchemy for database operations
- **Database:** SQLite (`server/dogshelter.db`)
- **Models:** Located in `server/models/` with validation in model classes
- **Key patterns:**
  - Models inherit from `BaseModel` (see `server/models/base.py`)
  - Use `@validates` decorators for field validation
  - Enums for status fields (e.g., `AdoptionStatus`)
  - All API routes return JSON via `jsonify()`
  - Type hints required: `def get_dog(id: int) -> Response:`

## Frontend (Astro + Svelte)

- **Framework:** Astro for pages, Svelte for interactive components
- **Styling:** TailwindCSS v4 with dark mode enabled by default
- **TypeScript:** Always use arrow functions, not `function` keyword
- **Key patterns:**
  - Pages in `client/src/pages/*.astro`
  - Dynamic components in `client/src/components/*.svelte`
  - Dark theme with slate color palette (see `Layout.astro`)
  - Client-side hydration: `<Component client:only="svelte" />`

## Development Workflow

**Starting the application:**
```bash
# From project root, starts both servers
./scripts/start-app.sh  # Port 5100 (Flask), Port 4321 (Astro)
```

**Testing:**
- Backend: `cd server && python -m unittest` (requires activated venv)
- Frontend E2E: `cd client && npm run test:e2e` (Playwright tests)
- Backend tests use mocking (`unittest.mock`) - see `server/test_app.py`

**Virtual environment setup:**
```bash
# Python venv automatically created by start-app.sh
source ./venv/bin/activate  # Manual activation if needed
```

## Project-Specific Conventions

1. **No generic error handling comments** - Follow existing error patterns
2. **Database queries join Dog + Breed** - See `app.py` route examples
3. **Status fields use Enums** - Convert to `.name` for JSON (e.g., `status.name`)
4. **Frontend uses modern practices** - Arrow functions, Svelte 5, TypeScript
5. **Workshop content is immutable** - Never modify files in `content/` directory

## Integration Points

- **API base URL:** `http://localhost:5100/api`
- **Frontend dev URL:** `http://localhost:4321`
- **Database init:** Auto-creates tables on first run via `models/__init__.py`
- **Seeding data:** Use `./scripts/seed-database.sh` for test data
