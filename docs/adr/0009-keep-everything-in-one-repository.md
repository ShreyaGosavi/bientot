# 9. Keep everything in one repository

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Bientôt has a frontend, backend, ML code and docs.

## Decision

Use one repository (a monorepo) with independent top-level folders: `frontend/`, `backend/`, `ml/` and `docs/`. Each has its own dependencies and tests.

## Alternatives considered

- Separate repositories per part: cleaner boundaries, but much more overhead for a solo project

## Consequences

- One place for code, docs, CI and issues
- Shared tooling (hooks, formatting) set up once at the root
