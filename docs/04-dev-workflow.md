# Dev Workflow

How work moves from an idea to `main`.

## Branches

| Branch                                 | Purpose                                       |
| -------------------------------------- | --------------------------------------------- |
| `main`                                 | Stable, released code only                    |
| `dev`                                  | Integration branch, and the default branch    |
| `feat/…`, `fix/…`, `docs/…`, `chore/…` | Short-lived work branches, created from `dev` |

```text
feat/plan-my-day ──PR (squash)──▶ dev ──PR (merge)──▶ main
```

## Commits

Every commit follows [Conventional Commits](https://www.conventionalcommits.org):

| Type        | When                        |
| ----------- | --------------------------- |
| `feat:`     | A new feature               |
| `fix:`      | A bug fix                   |
| `docs:`     | Documentation               |
| `chore:`    | Setup, config, tooling      |
| `refactor:` | Code change, same behaviour |
| `test:`     | Tests                       |
| `ci:`       | GitHub Actions              |
| `style:`    | Formatting only             |

Example: `feat: add plan my day checks`

## What runs on every commit

Husky runs these automatically:

1. **Gitleaks** scans staged files for secrets
2. **lint-staged** formats and lints only the changed files (Prettier, markdownlint, and later ESLint and Ruff)
3. **Commitlint** checks the commit message

If any step fails, the commit is blocked.

## Pull requests

1. Push the branch and open a PR into `dev`
2. Fill in the PR template (what, why, checklist)
3. CI runs: **Format & docs**, **Commit messages**, **Secret scan**
4. All checks green → **Squash and merge**
5. The branch is deleted automatically

## Releases

When `dev` is stable, open a PR from `dev` into `main` and use **Create a merge commit**, so the release keeps its full history.

## Branch protection

| Rule                      | `main` | `dev`  |
| ------------------------- | ------ | ------ |
| Changes only through PRs  | ✅     | ✅     |
| Required checks must pass | ✅     | ✅     |
| Force pushes blocked      | ✅     | ✅     |
| Deletion blocked          | ✅     | ✅     |
| Allowed merge method      | Merge  | Squash |

Nobody can bypass these rules, including the repo owner.

## Dependencies

Dependabot checks for updates every week and opens PRs into `dev`.

## Day-to-day commands

```bash
git checkout dev && git pull          # start from the latest dev
git checkout -b feat/short-name       # new branch
git add . && git commit -m "feat: …"  # commit (hooks run)
git push -u origin feat/short-name    # push and open a PR
```
