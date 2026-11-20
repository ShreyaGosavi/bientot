# 18. Use uv for Python packages and environments

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

The backend and ML need reproducible Python environments that don't clash with other projects.

## Decision

Use uv, with `pyproject.toml` and a `uv.lock` lockfile in each Python folder.

## Alternatives considered

- pip with requirements.txt: familiar, but slower and without a proper lockfile
- Conda: already installed, but heavier and less common for app projects

## Consequences

- The same package versions everywhere: locally, in CI and on the server
- Each project gets its own isolated environment
