# 11. Keep moved tasks as one task

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Tasks are often moved to another day. This history matters for the completion rate.

## Decision

A moved task keeps the same record with a new date. What happened each day (done, not done, moved) is stored in a separate `task_outcomes` table.

## Alternatives considered

- Create a new copy each time a task moves: simple, but splits history and risks counting a task twice when learning

## Consequences

- One task, one actual time, one learning example
- The full history of moves is kept
