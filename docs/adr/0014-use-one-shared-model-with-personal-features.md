# 14. Use one shared model with personal features

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Most users won't have enough data for a model of their own, especially early on.

## Decision

Train one model on all users' data, with personal features (the user's own category and label factors) so each prediction is based on that user's history.

## Alternatives considered

- A separate model per user: very personal, but useless until each person has a lot of data

## Consequences

- New users get sensible predictions sooner
- A per-user correction can be added later for users with long histories
