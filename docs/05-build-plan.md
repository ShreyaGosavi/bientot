# Build Plan

Bientôt is built in phases. Each phase works on its own, so the app is usable from early on and the ML is added on top.

## Overview

| Phase | Focus               | Result                                    |
| ----- | ------------------- | ----------------------------------------- |
| 0     | Foundation          | Repo, tooling and docs ready              |
| 1     | Backend basics      | Users and tasks work through the API      |
| 2     | Fallback prediction | Predictions and verdicts without ML       |
| 3     | Machine learning    | A trained model serves predictions        |
| 4     | Smart planning      | Plan my day, the checks and learning data |
| 5     | Frontend            | The full app on screen                    |
| 6     | Dashboard           | Patterns and progress visible             |
| 7     | Launch              | Live and documented                       |
| v2    | Insights            | When you perform best, retraining         |

## Phase 0: Foundation

- [x] Repository, branches and branch protection
- [x] Code quality and Git hooks
- [x] CI with GitHub Actions
- [ ] Project docs
- [ ] Data design

## Phase 1: Backend basics

- [ ] FastAPI project set up with uv, Ruff, mypy and pytest
- [ ] PostgreSQL connected, with Alembic migrations
- [ ] Sign up and login (JWT)
- [ ] Add, edit, delete and list tasks

## Phase 2: Fallback prediction

- [ ] Category factors calculated from past tasks
- [ ] Prediction = estimate × category factor
- [ ] Verdicts: need more, about right, might take less
- [ ] 7-day cold period
- [ ] Every prediction saved with its source

_At the end of this phase, Bientôt already works without any ML._

## Phase 3: Machine learning

- [ ] Synthetic training data generated
- [ ] Baseline model trained and evaluated
- [ ] Stronger models compared against the baseline
- [ ] Prediction service built
- [ ] Backend connected to the model, with the fallback kept
- [ ] Experiments tracked

## Phase 4: Smart planning

- [ ] Plan my day
- [ ] The three checks: time, count and load
- [ ] Healthy limits
- [ ] Daily outcomes and completion rate
- [ ] Overrun reasons and feelings after tasks

## Phase 5: Frontend

- [ ] Next.js project set up with ESLint, Vitest and Tailwind
- [ ] Brand styles: sage, sand, Instrument Serif, Lexend
- [ ] Screens: landing, onboarding, today, add task, finish task
- [ ] Connected to the backend

## Phase 6: Dashboard

- [ ] Prediction accuracy
- [ ] Completion rate
- [ ] Where your time goes
- [ ] Your guesses vs reality

## Phase 7: Launch

- [ ] Docker and docker-compose
- [ ] Hosting chosen and deployed
- [ ] Setup guide and API reference written
- [ ] README finished

## v2

- [ ] When you perform best
- [ ] Retraining on real user data

## Why this order

- **Backend before frontend:** the rules and data come first; screens just show them.
- **Fallback before ML:** the app never depends on the model.
- **Each phase builds on the last:** nothing is built on parts that don't exist yet.
