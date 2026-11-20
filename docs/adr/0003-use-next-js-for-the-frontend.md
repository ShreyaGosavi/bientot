# 3. Use Next.js for the frontend

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Bientôt needs a web frontend with several screens and easy deployment.

## Decision

Build the frontend with Next.js, React and TypeScript.

## Alternatives considered

- React with Vite: lighter, but needs a separate router and has no built-in deployment story

## Consequences

- File-based routing replaces a separate router
- TypeScript catches errors before the code runs
- Easy to deploy later
