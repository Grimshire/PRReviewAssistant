# Project context

Repository: /c/Users/TurnerAdam/Documents/Projects/cloned/slate-react/slate-react
Generated: 2026-11-20 14:09:56 +0100

This is a source-grounded project briefing assembled from repository documentation, instructions, manifests, and a shallow directory map. It is not a substitute for reading the changed code. Files are included verbatim, subject to size limits.

## Git overview

Configured remote names (URLs are intentionally omitted to avoid exposing embedded credentials):
  origin

## Top-level structure

```text
.gitattributes
.gitignore
.semgrepignore
azure-pipeline.yml
backend
backend/.editorconfig
backend/.gitignore
backend/azure-pipelines-python-development.yml
backend/azure-pipelines-python-master.yml
backend/azure-pipelines-python-qa.yml
backend/DEPLOYMENT.md
backend/python
backend/README.md
frontend
frontend/.env.example
frontend/.gitignore
frontend/biome.json
frontend/components.json
frontend/e2e
frontend/eslint.config.js
frontend/index.html
frontend/package.json
frontend/package-lock.json
frontend/playwright.config.ts
frontend/public
frontend/README.e2e.md
frontend/README.md
frontend/src
frontend/tsconfig.app.json
frontend/tsconfig.json
frontend/tsconfig.node.json
frontend/vite.config.ts
frontend/vitest.config.ts
package.json
package-lock.json
README.md
```

## Discovered project files

The following files are included below. Sensitive environment files are intentionally excluded.

| File | Bytes | Included |
|---|---:|---|
| `README.md` | 3159 | Yes, subject to limits |
| `package.json` | 468 | Yes, subject to limits |
| `backend/python/pyproject.toml` | 2897 | Yes, subject to limits |
| `frontend/package.json` | 3573 | Yes, subject to limits |
| `frontend/tsconfig.app.json` | 833 | Yes, subject to limits |
| `frontend/tsconfig.json` | 133 | Yes, subject to limits |
| `frontend/tsconfig.node.json` | 694 | Yes, subject to limits |

### README.md

````text
# Slate

A scheduler-focused web app that aggregates events from multiple enterprise providers (Microsoft Graph, Exchange, CRM, SharePoint, ServiceNow, JPAD) into centrally managed calendar views.

## Structure

```
slate/
├── frontend/     React 19 + TypeScript + Vite (UI)
├── backend/      Python 3.12+ + FastAPI (API)
└── package.json  npm workspace root (run both with npm run dev)
```

Full project docs are in each workspace:

- [frontend/README.md](./frontend/README.md)
- [backend/README.md](./backend/README.md)

## Quick Start

### Prerequisites

- Node.js 22+
- Python 3.12+
- PostgreSQL running locally — see [backend/README.md](./backend/README.md) for database setup

### 1. Configure the backend

On first run the script will create `backend/python/.env` from the example and exit. Edit it before continuing:

```env
DATABASE_URL=postgresql+asyncpg://slate_dev_user:change_me@localhost:5432/slate_dev
ENVIRONMENT=development
PROVIDERS_MOCK_ENABLED=true
AUTH_BYPASS_ENABLED=true
USER_IDENTITY_SOURCE=org_chart
ORG_CHART_API_URL=http://localhost:8001
```

### 2. Configure the frontend

Copy the frontend env template and fill in your OIDC values:

```bash
cp frontend/.env.example frontend/.env.development
# Edit frontend/.env.development and set VITE_OIDC_AUTHORITY and VITE_OIDC_CLIENT_ID
```

### 3. Start both servers

```bash
npm install
npm run dev
```

| Service  | URL                          |
|----------|------------------------------|
| Frontend | http://localhost:5173         |
| Backend  | http://localhost:8000/docs   |

On first run the backend will install Python dependencies and run database migrations before starting — this takes a minute. Subsequent starts are fast.

## Stack

| Layer    | Technology                                      |
|----------|-------------------------------------------------|
| Frontend | React 19, TypeScript, Vite 8, Tailwind CSS 4, Radix UI, Zustand, React Router 7 |
| Backend  | FastAPI, SQLAlchemy (async), PostgreSQL, Alembic, Pydantic v2 |
| Auth     | OIDC / JWT (Microsoft Entra), org-chart API or local table for user identity |
| Providers | Microsoft Graph, Exchange, CRM, SharePoint, ServiceNow, JPAD |

## Code Quality

The pipeline runs four quality gates. All can be run locally before pushing.

### Frontend — Biome

```bash
# Lint
npm run lint

# Format
npm run format
```

### Backend — Ruff & Black

From `backend/python/`:

```powershell
# Format check
.\.venv\Scripts\black --check .

# Auto-format
.\.venv\Scripts\black .

# Lint (report only)
.\.venv\Scripts\ruff check .

# Lint + auto-fix
.\.venv\Scripts\ruff check --fix .
```

See [backend/README.md](./backend/README.md#code-quality) for full details including Semgrep (SAST) and TruffleHog (secret scanning).

## Related Projects

| Project       | Role                                                              |
|---------------|-------------------------------------------------------------------|
| `org-chart`   | Optional personnel identity source of truth (backend calls it internally)  |
| `infra-proxy` | nginx / IIS reverse proxy config for HTTPS and `/api` routing     |

````

### package.json

````text
{
  "name": "slate",
  "private": true,
  "workspaces": [
    "frontend"
  ],
  "scripts": {
    "dev": "concurrently --names \"frontend,backend\" --prefix-colors \"cyan,yellow\" \"npm run dev --workspace=frontend\" \"bash backend/python/run_dev.sh\"",
    "build": "npm run build --workspace=frontend",
    "lint": "npm run lint --workspace=frontend",
    "format": "npm run format --workspace=frontend"
  },
  "devDependencies": {
    "concurrently": "^9.0.0"
  }
}

````

### backend/python/pyproject.toml

````text
[build-system]
requires = ["setuptools>=72"]
build-backend = "setuptools.build_meta"

[project]
name = "slate-api"
version = "2.0.0"
description = "SOCEUR / ISKM Combined Calendar API (Python/FastAPI)"
requires-python = ">=3.12"
dependencies = [
    # Web framework
    "fastapi>=0.115",
    "uvicorn[standard]>=0.30",

    # Database
    "sqlalchemy[asyncio]>=2.0",
    "asyncpg>=0.29",
    "alembic>=1.13",

    # Validation & settings
    "pydantic>=2.7",
    "pydantic-settings>=2.3",
    "email-validator>=2.2",

    # Auth
    "python-jose[cryptography]>=3.3",
    "cryptography>=42",

    # HTTP (provider calls)
    "httpx>=0.27",

    # Providers
    "msgraph-sdk>=1.5",
    "azure-identity>=1.17",
    "exchangelib>=5.3",
    "tenacity>=8.5",
    "holidays>=0.98",

    # PDF
    "playwright>=1.44",

    # Email
    "aiosmtplib>=3.0",

    # Logging
    "structlog>=24.2",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.2",
    "pytest-asyncio>=0.23",
    "pytest-cov>=5.0",
    "httpx>=0.27",
    "anyio[trio]>=4.3",
    "aiosqlite>=0.20",
    "cryptography>=42",
    # Linting & formatting
    "ruff>=0.9",
    "black>=25.0",
]

[tool.setuptools.packages.find]
where = ["."]
include = ["app*"]

[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
addopts = "--cov=app --cov-report=term-missing"

[tool.coverage.run]
source = ["app"]
omit = ["tests/*"]

# ---------------------------------------------------------------------------
# Ruff — linter (replaces flake8, isort, pyupgrade, bandit, and more)
# ---------------------------------------------------------------------------
[tool.ruff]
target-version = "py312"
line-length = 88
extend-exclude = ["migrations"]

[tool.ruff.lint]
select = [
    "E",     # pycodestyle errors
    "W",     # pycodestyle warnings
    "F",     # pyflakes
    "I",     # isort
    "B",     # flake8-bugbear
    "C4",    # flake8-comprehensions
    "UP",    # pyupgrade
    "S",     # flake8-bandit (security)
    "N",     # pep8-naming
    "ASYNC", # flake8-async
    "RUF",   # ruff-specific rules
]
ignore = [
    "E501",  # line too long — Black enforces line length
    "S101",  # use of assert — acceptable in tests
    "B008",  # Depends() in default args — FastAPI's standard DI pattern
]

[tool.ruff.lint.per-file-ignores]
"tests/**/*.py" = ["S101", "S105", "S106"]
"scripts/**/*.py" = ["S101"]
# SQLAlchemy ORM string-based relationship() references are cross-module forward
# refs that cannot be imported without creating circular imports.
"app/models/*.py" = ["F821"]

[tool.ruff.lint.isort]
known-first-party = ["app"]

# ---------------------------------------------------------------------------
# Black — code formatter
# ---------------------------------------------------------------------------
[tool.black]
line-length = 88
target-version = ["py312"]
extend-exclude = '''
(
  migrations/
)
'''

````

### frontend/package.json

````text
{
    "name": "slate",
    "private": true,
    "version": "0.0.0",
    "type": "module",
    "scripts": {
        "dev": "vite",
        "build": "tsc -b && vite build",
        "build:staging": "tsc -b && vite build --mode staging",
        "build:qa": "tsc -b && vite build --mode qa",
        "build:prod": "tsc -b && vite build --mode production",
        "preview": "vite preview",
        "lint": "biome lint .",
        "lint:fix": "biome lint --write .",
        "format": "biome format --write .",
        "check": "biome check .",
        "check:fix": "biome check --write .",
        "test": "vitest",
        "test:ui": "vitest --ui",
        "test:run": "vitest run --testTimeout=30000 --hookTimeout=30000",
        "test:coverage": "vitest run --coverage",
        "test:e2e": "npx playwright test",
        "test:e2e:ui": "npx playwright test --ui",
        "test:e2e:smoke": "npx playwright test --grep @smoke",
        "test:e2e:full": "npx playwright test --grep-invert @nightly-only",
        "test:e2e:nightly": "npx playwright test"
    },
    "dependencies": {
        "@hookform/resolvers": "^5.2.2",
        "@lexical/html": "^0.45.0",
        "@lexical/link": "^0.45.0",
        "@lexical/list": "^0.45.0",
        "@lexical/react": "^0.45.0",
        "@lexical/rich-text": "^0.45.0",
        "@lexical/selection": "^0.45.0",
        "@lexical/utils": "^0.45.0",
        "@radix-ui/react-alert-dialog": "^1.1.15",
        "@radix-ui/react-checkbox": "^1.3.3",
        "@radix-ui/react-collapsible": "^1.1.12",
        "@radix-ui/react-dialog": "^1.1.15",
        "@radix-ui/react-dropdown-menu": "^2.1.16",
        "@radix-ui/react-label": "^2.1.8",
        "@radix-ui/react-popover": "^1.1.15",
        "@radix-ui/react-scroll-area": "^1.2.10",
        "@radix-ui/react-select": "^2.2.6",
        "@radix-ui/react-separator": "^1.1.8",
        "@radix-ui/react-slot": "^1.2.4",
        "@radix-ui/react-switch": "^1.2.6",
        "@radix-ui/react-tabs": "^1.1.13",
        "@radix-ui/react-tooltip": "^1.2.8",
        "@tanstack/react-table": "^8.21.3",
        "axios": "^1.15.2",
        "class-variance-authority": "^0.7.1",
        "clsx": "^2.1.1",
        "date-fns": "^4.1.0",
        "dompurify": "^3.4.8",
        "handlebars": "^4.7.9",
        "lexical": "^0.45.0",
        "lucide-react": "^1.14.0",
        "next-themes": "^0.4.6",
        "react": "^19.2.5",
        "react-big-calendar": "^1.19.4",
        "react-calendar": "^6.0.1",
        "react-dom": "^19.2.5",
        "react-hook-form": "^7.74.0",
        "react-router-dom": "^7.14.2",
        "sonner": "^2.0.7",
        "tailwind-merge": "^3.5.0",
        "zod": "^4.4.1",
        "zustand": "^5.0.12"
    },
    "devDependencies": {
        "@biomejs/biome": "^2.4.13",
        "@playwright/test": "^1.60.0",
        "@tailwindcss/vite": "^4.2.4",
        "@testing-library/jest-dom": "^6.9.1",
        "@testing-library/react": "^16.3.2",
        "@testing-library/user-event": "^14.6.1",
        "@types/dompurify": "^3.0.5",
        "@types/handlebars": "^4.0.40",
        "@types/node": "^24.13.2",
        "@types/react": "^19.2.14",
        "@types/react-big-calendar": "^1.16.3",
        "@types/react-dom": "^19.2.3",
        "@vitejs/plugin-react": "^6.0.1",
        "@vitest/coverage-v8": "^4.1.8",
        "@vitest/ui": "^4.1.8",
        "jsdom": "^29.1.1",
        "playwright": "^1.60.0",
        "tailwindcss": "^4.2.4",
        "tailwindcss-animate": "^1.0.7",
        "typescript": "~6.0.2",
        "vite": "^8.0.10",
        "vitest": "^4.1.8"
    }
}

````

### frontend/tsconfig.app.json

````text
{
    "compilerOptions": {
        "incremental": true,
        "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.app.tsbuildinfo",
        "target": "ES2020",
        "lib": ["ES2020", "DOM", "DOM.Iterable"],
        "module": "ESNext",
        "skipLibCheck": true,

        /* Bundler mode */
        "moduleResolution": "bundler",
        "allowImportingTsExtensions": true,
        "verbatimModuleSyntax": true,
        "moduleDetection": "force",
        "noEmit": true,
        "jsx": "react-jsx",

        /* Linting */
        "strict": true,
        "noUnusedLocals": true,
        "noUnusedParameters": true,
        "noFallthroughCasesInSwitch": true,
        "forceConsistentCasingInFileNames": true,

        /* Path aliases */
        "paths": {
            "@/*": ["./src/*"]
        }
    },
    "include": ["src"]
}

````

### frontend/tsconfig.json

````text
{
    "files": [],
    "references": [
        { "path": "./tsconfig.app.json" },
        { "path": "./tsconfig.node.json" }
    ]
}

````

### frontend/tsconfig.node.json

````text
{
    "compilerOptions": {
        "incremental": true,
        "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.node.tsbuildinfo",
        "target": "es2023",
        "lib": ["ES2023"],
        "module": "esnext",
        "types": ["node"],
        "skipLibCheck": true,

        /* Bundler mode */
        "moduleResolution": "bundler",
        "allowImportingTsExtensions": true,
        "verbatimModuleSyntax": true,
        "moduleDetection": "force",
        "noEmit": true,

        /* Linting */
        "noUnusedLocals": true,
        "noUnusedParameters": true,
        "erasableSyntaxOnly": true,
        "noFallthroughCasesInSwitch": true
    },
    "include": ["vite.config.ts"]
}

````

