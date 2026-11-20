# 13. Predict the error ratio, not minutes

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Tasks range from minutes to hours, but estimation habits ("I always take 40% longer") are similar across sizes.

## Decision

The model predicts the ratio of actual to estimate. Prediction = estimate × predicted ratio.

## Alternatives considered

- Predict minutes directly: intuitive, but the model would mostly learn task size rather than the habit

## Consequences

- The model learns the habit, which generalises across short and long tasks
- Easy to compare with the fallback, which is also a ratio
