# 17. Use Tailwind CSS for styling

- **Status:** Accepted
- **Date:** 2026-11-20

## Context

The frontend needs consistent styling based on the brand colours and fonts.

## Decision

Use Tailwind CSS, with the brand colours and fonts set up as tokens.

## Alternatives considered

- CSS Modules: plain CSS per component, but slower to build with and easier to drift from the brand

## Consequences

- Brand values are used consistently (`bg-sage`, `font-serif`)
- Fast to build screens
