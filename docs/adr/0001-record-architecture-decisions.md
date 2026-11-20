# 1. Record architecture decisions

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Bientôt is a solo project that will grow over months. Decisions made now (and the reasons behind them) are easy to forget.

## Decision

Record every significant decision as an Architecture Decision Record (ADR) in `docs/adr/`, one file per decision, numbered in order.

## Alternatives considered

- A single decision log file: simpler, but harder to link to and review one decision at a time
- No record: fastest, but the reasons are lost

## Consequences

- Every decision has a short, linkable explanation
- A decision is never edited after it's accepted; a new ADR replaces it instead
