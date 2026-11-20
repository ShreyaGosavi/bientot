# 12. End the cold period after 7 active days

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Predictions need some personal history first. Counting from sign up would start predictions even for users with no data.

## Decision

Start predicting after 7 active days, meaning days with at least one finished task.

## Alternatives considered

- 7 calendar days from sign up: simpler, but a user who signs up and returns later would get predictions with no data

## Consequences

- Predictions only start once there is real personal data
