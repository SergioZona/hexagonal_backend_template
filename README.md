# hexagonal-backend-template

> Production-ready Python **Hexagonal Architecture (Ports & Adapters)** template.
> Built with UV, FastAPI, PostgreSQL, and GitHub Actions CI/CD.

---

## Tech Stack

| Concern | Tool |
|---|---|
| Language | Python 3.14 |
| Package manager | UV |
| Framework | FastAPI |
| Database | PostgreSQL (asyncpg + SQLAlchemy async) |
| DI Container | dependency-injector |
| Response format | JSend |
| Linter + Formatter | Ruff (configured in `pyproject.toml`) |
| Type checker | mypy |
| Testing | pytest + pytest-asyncio + httpx |
| Architecture tests | import-linter |
| Security | bandit + pip-audit |
| Code quality | SonarCloud (free tier, main branch) |
| Versioning | Release Please (conventional commits) |

---

## Setup

```bash
# Install all dependencies
uv sync --group dev
```

Copy `src/env/.env` is already the local default — it loads automatically when `APP_ENV` is not set.
Fill in your local secrets there (it is gitignored and will never be committed):

```ini
# src/env/.env  — your local secrets (gitignored)
DATABASE_PASSWORD=your-local-password
SECRET_KEY=your-local-secret
```

> **Secrets are never in named env files** (`DEV.env`, `TEST.env`, etc.).
> They are always injected at runtime: locally via `src/env/.env`, in CI via GitHub Actions secrets, in production via Dokploy.

---

## Running the Dev Server

### PowerShell
```powershell
uv run uvicorn app.infrastructure.main:app --reload --host 0.0.0.0 --port 8000
```

### WSL / bash
```bash
uv run uvicorn app.infrastructure.main:app --reload --host 0.0.0.0 --port 8000
```

`src/env/.env` is loaded automatically. Server starts at `http://localhost:8000`.

| URL | Description |
|---|---|
| `http://localhost:8000/docs` | Swagger UI |
| `GET /health` | Liveness probe |
| `GET /ready` | Readiness probe |
| `POST /ping` | Ping → Pong |
| `GET /api/v1/items` | List items |

---

## Tests

See [`tests/README.md`](tests/README.md) for the full test guide.

Quick run (unit tests only, no secrets or DB needed for local defaults):
```bash
uv run pytest tests/unit/ -v
```

---

## Code Quality

Ruff is the single tool for linting **and** formatting (configured in `pyproject.toml`):

```bash
uv run ruff check src/ tests/      # lint
uv run ruff format src/ tests/     # format (auto-fix)
uv run ruff format --check src/ tests/  # format check only (CI)
uv run mypy src/                   # type check
```

---

## Security

```bash
uv run bandit -r src/ -ll          # static security analysis
uv run pip-audit                   # known CVE check on dependencies
```

---

## Project Structure

```
src/
├── env/            # Config files — .env (gitignored local), DEV/TEST/UAT/PROD.env
└── app/
    ├── domain/         # Entities, value objects, exceptions
    ├── application/    # Ports (ABCs) + use cases
    └── infrastructure/ # Adapters (HTTP, Postgres) + config + DI container

tests/              # See tests/README.md
docs/
├── CONSTITUTION.md # Project law — read this first
└── getting_started.md

.agents/            # AI agent context (rules, skills)
```

---

## Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/health` | Liveness probe |
| GET | `/ready` | Readiness probe |
| POST | `/ping` | Ping → Pong |
| POST | `/api/v1/items` | Create item |
| GET | `/api/v1/items` | List all items |
| GET | `/api/v1/items/{id}` | Get item by id |
| PATCH | `/api/v1/items/{id}` | Update item |
| DELETE | `/api/v1/items/{id}` | Delete item |

---

## License

MIT
