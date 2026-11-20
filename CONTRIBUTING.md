# Contributing

## Branches

| Branch                                 | Purpose                                           |
| -------------------------------------- | ------------------------------------------------- |
| `main`                                 | Stable, released code only                        |
| `dev`                                  | Integration branch; all features merge here first |
| `feat/…`, `fix/…`, `docs/…`, `chore/…` | One small task each, branched from `dev`          |

Examples: `feat/plan-my-day`, `fix/verdict-percent`, `docs/build-plan`

## Commits

Follow [Conventional Commits](https://www.conventionalcommits.org): `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`

## Flow

1. Branch from `dev`: `git checkout dev && git checkout -b feat/xyz`
2. Commit your work (hooks check it automatically)
3. Push and open a PR into `dev`
4. CI must pass before merging
5. When `dev` is stable, open a PR from `dev` into `main` (a release)
