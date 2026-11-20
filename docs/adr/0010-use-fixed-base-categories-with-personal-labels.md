# 10. Use fixed base categories with personal labels

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

Users name tasks in their own words, but the model needs consistent groups to learn from. Bientôt must suit anyone, not only students.

## Decision

Use a fixed list of base categories (Deep focus, Learning, Creative, Admin, Communication, Errands & chores, Personal, Miscellaneous). Users create any number of their own labels, each mapped to one category.

## Alternatives considered

- Free-form categories only: personal, but impossible to learn from consistently
- Fixed categories only: consistent, but impersonal

## Consequences

- The model learns from consistent categories while users keep their own words
- Deep focus and Learning are marked as heavy, which drives the load check
