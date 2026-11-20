# 16. Build JWT authentication in-house

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Users need to sign up and log in.

## Decision

Build login with JWT tokens in the FastAPI backend, storing only password hashes.

## Alternatives considered

- A hosted auth service (such as Clerk or Supabase Auth): faster to set up, but hides how authentication works

## Consequences

- A deeper understanding of authentication
- Security (hashing, token expiry) is our own responsibility
