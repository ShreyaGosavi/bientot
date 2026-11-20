# 5. The backend is the only middleman

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Bientôt has four parts: frontend, backend, ML service and database. Without a rule, any part could call any other.

## Decision

All requests go through the backend. The frontend never talks to the database or the model, and the ML service never calls anyone.

## Alternatives considered

- Let the frontend call the ML service directly: slightly faster, but rules would be split across two places

## Consequences

- All rules (checks, verdicts, fallback) live in one place
- Each part can change without breaking the others
