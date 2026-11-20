# 15. Evaluate the model against baselines with a time-based split

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

A model needs a clear bar to decide whether it is good enough to use.

## Decision

The main metric is the % of predictions within ±15% of the actual time, with average error in minutes (MAE) also tracked. Data is split by time (train on older, test on newer). The model must beat two baselines: trusting the user's estimate, and the fallback formula.

## Alternatives considered

- Random train/test split: easier, but lets the model see the future and overstates accuracy

## Consequences

- The main metric matches the product's own ±15% verdict rule
- A model that doesn't beat simple rules is never shipped
