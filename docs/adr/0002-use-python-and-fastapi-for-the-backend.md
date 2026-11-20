# 2. Use Python and FastAPI for the backend

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

The ML is written in Python and the frontend is Next.js. The backend needs to talk to both.

## Decision

Build the backend in Python with FastAPI.

## Alternatives considered

- Node (Express or NestJS): shows both languages, but means three services in two languages and more complex backend–ML calls
- Next.js API routes only: fewest parts, but mixes frontend and backend in one place

## Consequences

- The backend and ML share one language, tools (uv, Ruff, mypy, pytest) and code
- Node is still part of the stack through Next.js
- FastAPI generates API docs automatically
