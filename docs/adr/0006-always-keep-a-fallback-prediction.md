# 6. Always keep a fallback prediction

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

The ML service can fail, be slow, or not exist yet in early phases.

## Decision

When the model is unavailable, the backend predicts with a formula: estimate × the user's category factor. Every prediction records whether it came from the model or the fallback.

## Alternatives considered

- Show no prediction when the model is down: simpler, but the app breaks whenever the model does

## Consequences

- The app never depends on the model
- Bientôt works before any ML is built
- The model can be measured against the fallback
