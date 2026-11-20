# 8. Enforce conventional commits, formatting and secret scanning

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Inconsistent commit messages and formatting make history hard to read, and a leaked secret is costly.

## Decision

Use Husky, lint-staged, Prettier, markdownlint, Commitlint and Gitleaks on every commit, and run the same checks in GitHub Actions on every push and pull request.

## Alternatives considered

- Hooks only: can be skipped with `--no-verify`
- CI only: problems are found later, after pushing

## Consequences

- Problems are caught locally first, and CI is the safety net
- Commit messages can later generate the changelog automatically
