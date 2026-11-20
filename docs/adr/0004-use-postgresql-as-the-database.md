# 4. Use PostgreSQL as the database

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Bientôt stores related data: users, labels, tasks, predictions and outcomes.

## Decision

Use PostgreSQL, accessed through SQLAlchemy, with Alembic for migrations.

## Alternatives considered

- A document database: more flexible, but the data is clearly relational

## Consequences

- Relationships and constraints (unique labels, one outcome per day) are enforced by the database
- Schema changes are tracked as migrations
