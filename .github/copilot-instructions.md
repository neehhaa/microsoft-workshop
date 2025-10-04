# Dog Shelter Workshop Application

This is a monorepo application for a dog adoption shelter, built as a learning project for GitHub Copilot workshops. The application consists of a Flask backend API and an Astro+Svelte frontend.

## Project Structure

- **server/**: Flask backend API with SQLAlchemy ORM (runs on port 5100)
- **client/**: Astro frontend with Svelte components (runs on port 4321)
- **scripts/**: Shell scripts for setup, database seeding, and app startup
- **content/**: Workshop tutorial materials and exercises

## Backend (Flask + SQLAlchemy)

- **Python version**: 3.x with virtual environment (venv/.venv)
- **Always use type hints** for function signatures and variables (e.g., `def get_dogs() -> Response:`)
- **Database**: SQLite with SQLAlchemy ORM
  - Models use enum types for status fields (e.g., `AdoptionStatus` enum in `Dog` model)
  - Models inherit from `BaseModel` which provides validation helpers
  - Use `db.session.query()` for database queries with explicit column selection
  - Join tables to get related data (e.g., `Dog` joins with `Breed` to get breed name)
- **API Routes**: RESTful endpoints under `/api/` prefix
  - Return JSON responses using `jsonify()`
  - Use proper HTTP status codes (e.g., 404 for not found)
  - Route decorators specify methods: `@app.route('/api/dogs', methods=['GET'])`
- **Testing**: Use `unittest` with `unittest.mock` for mocking database queries
  - Tests are in `server/test_app.py`
  - Run with: `python -m pytest test_app.py -v`

## Frontend (Astro + Svelte)

- **Framework**: Astro with SSR mode (`output: 'server'`)
- **Components**: Svelte 5 for interactive components
- **Styling**: Tailwind CSS v4 with dark mode theme
  - Use dark slate colors: `bg-slate-800`, `text-slate-100`, `border-slate-700`
  - Hover effects: `hover:border-blue-500/50`, `hover:translate-y-[-6px]`
- **TypeScript**: Always use arrow functions, not the `function` keyword
- **API Integration**: Middleware in `src/middleware.ts` forwards `/api/*` requests to Flask backend
  - Frontend fetches from `/api/dogs` which proxies to `http://localhost:5100/api/dogs`
- **Testing**: Playwright for E2E tests in `client/e2e-tests/`
  - Run with: `npm run test:e2e`

## Development Workflows

**Start the application**: `./scripts/start-app.sh` (starts both Flask and Astro servers)
**Seed database**: `./scripts/seed-database.sh` (populates SQLite with sample data)
**Run backend tests**: `cd server && python -m pytest test_app.py -v`
**Run frontend tests**: `cd client && npm run test:e2e`

## Key Patterns

- **Monorepo coordination**: Frontend proxies API calls to backend via Astro middleware
- **Type safety**: Python uses type hints, TypeScript for frontend
- **Database queries**: Use SQLAlchemy query API with explicit column selection and joins
- **Component architecture**: Astro pages with Svelte components marked `client:only="svelte"`
- **Error handling**: API returns proper error responses with status codes and messages
- **Loading states**: Frontend shows skeleton loaders while fetching data
