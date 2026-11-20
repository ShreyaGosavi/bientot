# 7. Use a main, dev and feature branch workflow

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Work needs to be reviewed and tested before reaching the stable version.

## Decision

`main` holds released code, `dev` is the integration branch, and work happens on short-lived branches from `dev`. Features are squash-merged into `dev`; `dev` is merge-committed into `main`. Both are protected by rulesets.

## Alternatives considered

- Trunk-based (everything into `main`): simpler, but no separate stable branch

## Consequences

- `main` is always stable
- `dev` has one clean commit per feature
- Nobody, including the owner, can push directly to `main` or `dev`
