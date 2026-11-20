# Tech Stack

Every tool in Bientôt, and why it was chosen.

## At a glance

| Layer    | Main tools                               |
| -------- | ---------------------------------------- |
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend  | Python, FastAPI, SQLAlchemy              |
| Database | PostgreSQL                               |
| ML       | scikit-learn, pandas, NumPy              |
| Tooling  | uv, npm, Docker, GitHub Actions          |

## Frontend

| Tool               | Why                                                         |
| ------------------ | ----------------------------------------------------------- |
| **Next.js**        | React framework with file-based routing and easy deployment |
| **React**          | Component-based UI, the industry standard                   |
| **TypeScript**     | Types catch bugs before the code runs                       |
| **Tailwind CSS**   | Fast styling; brand colours become tokens like `bg-sage`    |
| **TanStack Query** | Fetches and caches backend data cleanly                     |
| **Recharts**       | Charts for the dashboard                                    |

## Backend

| Tool           | Why                                                    |
| -------------- | ------------------------------------------------------ |
| **Python**     | Same language as the ML, so code and tools are shared  |
| **FastAPI**    | Fast, modern, and generates API docs automatically     |
| **Pydantic**   | Validates every request and response                   |
| **SQLAlchemy** | Works with the database using Python objects           |
| **Alembic**    | Tracks and applies database changes (migrations)       |
| **JWT auth**   | Login built from scratch, to understand how auth works |

## Database

| Tool           | Why                                                    |
| -------------- | ------------------------------------------------------ |
| **PostgreSQL** | Reliable, free, and the industry standard for app data |

## Machine learning

| Tool              | Why                                                    |
| ----------------- | ------------------------------------------------------ |
| **pandas, NumPy** | Cleaning and shaping data                              |
| **scikit-learn**  | Training and evaluating models                         |
| **FastAPI**       | A small service that serves predictions                |
| **MLflow**        | Tracks experiments and compares models _(added later)_ |
| **DVC**           | Versions datasets and trained models _(added later)_   |

**Approach:** start with a simple baseline model, then try stronger ones and keep a new model only if it measurably beats the baseline. The model choices and results will be documented in the ML approach doc.

## Code quality

| Tool             | Why                                               |
| ---------------- | ------------------------------------------------- |
| **Prettier**     | Formats JS, TS, JSON, YAML and Markdown           |
| **ESLint**       | Catches mistakes in the frontend code             |
| **Ruff**         | Lints and formats Python                          |
| **mypy**         | Type-checks Python                                |
| **markdownlint** | Keeps the docs clean                              |
| **EditorConfig** | Same indentation and line endings in every editor |

## Git workflow

| Tool            | Why                                                     |
| --------------- | ------------------------------------------------------- |
| **Husky**       | Runs checks automatically on every commit               |
| **lint-staged** | Checks only the files being committed, so it stays fast |
| **Commitlint**  | Enforces commit messages like `feat:`, `fix:`, `docs:`  |
| **Gitleaks**    | Blocks passwords and API keys from being committed      |

## Testing

| Tool       | Why                          |
| ---------- | ---------------------------- |
| **pytest** | Tests for the backend and ML |
| **Vitest** | Tests for the frontend       |

## Running the project

| Tool                 | Why                                                          |
| -------------------- | ------------------------------------------------------------ |
| **uv**               | Fast Python package and environment manager                  |
| **npm**              | Node package manager for the frontend and repo tools         |
| **Docker + Compose** | Runs the frontend, backend, ML and database with one command |
| **Makefile**         | Short commands like `make dev` and `make test`               |

## GitHub

| Tool               | Why                                                        |
| ------------------ | ---------------------------------------------------------- |
| **GitHub Actions** | CI: re-runs every check on each push and pull request      |
| **Rulesets**       | `main` and `dev` only accept changes through pull requests |
| **Dependabot**     | Opens pull requests to keep dependencies up to date        |
| **CodeQL**         | Scans the code for security issues                         |
| **release-please** | Writes the changelog and versions from commit messages     |

## Hosting

_To be decided once the app is ready to go live._
