# Repository Guidelines

## Project Structure & Module Organization
- Python FastAPI service. Main entry: `app/main.py` (Uvicorn).
- Key packages under `app/`:
  - `router/` (HTTP endpoints: Gemini/OpenAI, config, stats)
  - `service/` (business logic: chat, keys, tts, embeddings, files)
  - `database/` (SQLAlchemy models, connections, init)
  - `core/` (app factory, security, constants)
  - `config/`, `middleware/`, `domain/`, `handler/`, `utils/`
  - `templates/` and `static/` for UI assets

## Build, Test, and Development Commands
- Local run (recommended for development):
  - `pip install -r requirements.txt`
  - `uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload`
- Docker (quick start):
  - `docker-compose up -d` (uses `docker-compose.yml` and `.env`)
  - or `docker run -d -p 8000:8000 --env-file .env ghcr.io/snailyp/gemini-balance:latest`
- Health check: `GET /health`

## Coding Style & Naming Conventions
- Python 3.10+, 4-space indentation, UTF‑8.
- Use type hints on public functions and Pydantic models for request/response.
- Naming: `snake_case` for modules/functions, `PascalCase` for classes, `UPPER_SNAKE` for constants.
- Layout: routers only orchestrate; move business logic into `service/`; shared schemas in `domain/`.

## Testing Guidelines
- Preferred stack: `pytest` + `httpx`/`starlette.testclient` for FastAPI.
- Place tests under `tests/` mirroring `app/` (e.g., `tests/router/test_routes.py`).
- Name files `test_*.py`; aim for meaningful unit tests around services and handlers.
- For API tests, seed minimal config and assert status codes and payloads.

## Commit & Pull Request Guidelines
- Commits: concise, imperative style; prefer Conventional Commits (e.g., `feat:`, `fix:`, `docs:`).
- PRs: include purpose, scope, and screenshots for UI pages (templates). Link related issues, list breaking changes and migration steps if any.
- Keep PRs focused and small; add migration notes for DB changes in `app/database/`.

## Security & Configuration Tips
- Never commit secrets. Provide runtime values via `.env` (see `.env.example` in README) or Docker envs.
- Required vars: `API_KEYS`, `ALLOWED_TOKENS`; database via `DATABASE_TYPE` and `MYSQL_*` or `SQLITE_DATABASE`.
- Avoid logging sensitive values; use helpers in `app/log/logger.py` and handlers.

